<template>
<div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
      <MyHeader @addToDo="addToDo"/>
      <List :todos="todos"/>
      <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
    </div>
  </div>
</div>
</template>

<script>
import MyHeader from './components/MyHeader'
import MyFooter from './components/MyFooter'
import List from './components/List'

export default {
  name: 'App',
  components: {
    MyHeader,
    MyFooter,
    List
  },
  data(){
      return{
        todos:JSON.parse(localStorage.getItem('todos'))||[]//防止返回值是Null后报错
      }
    },
    methods:{
      //添加TODO
      addToDo(temp){
        this.todos.unshift(temp)
      },
      //修改todo状态
      changeToDo(id){
        this.todos.forEach(todo => {
          if(todo.id===id){
            todo.done=!todo.done//取反重新赋值
          }
        });
      },
      //删除一个todo
      deleteToDo(id){
        //过滤器搞完后要及时重新赋值回去,因为filter不影响原数组
        this.todos=this.todos.filter((todo)=>{
            return todo.id !==id
        })
      },
      //删除所有todo
      clearAllTodo(){
        this.todos=this.todos.filter((todo)=>{
            return !todo.done
        })
      },
      //修改所有的完成属性
      checkAllTodo(done){
        this.todos.forEach((todo)=>{
          todo.done=done
        })
      }
    },
    watch:{
      //简写版无法对数据的改变做出修改
      // todos(value){
      //   localStorage.setItem('todos',JSON.stringify(value))
      // }
      todos:{
        deep:true,//开启深度监视，完整版watch写法
        handler(value){
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    },
    mounted(){
      this.$bus.$on('changeToDo',this.changeToDo)
      this.$bus.$on('deleteToDo',this.deleteToDo)
    },
    beforeDestroy(){
      this.$bus.$off('changeToDo')
      this.$bus.$off('deleteToDo')
    }
}
</script>

<style>
  /*base*/
  body {
    background: #fff;
  }

  .btn {
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }

  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }

  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }

  .btn:focus {
    outline: none;
  }

  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }

</style>
