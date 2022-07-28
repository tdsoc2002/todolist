<template>
    <div class="todo-header">
        <input type="text"  placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
    </div>
</template>

<script>
import {nanoid} from 'nanoid'
export default {
    name:'MyHeader',
    methods:{
      add(e){

        //校验数据
        if(!e.target.value) return alert("请输入内容，别说啥事也没有！")
       //将用户输入的东西包装成todo对象
       const todoOBJ ={
          id:nanoid(),
          title:e.target.value,
          done:false
       }
       //让App组件添加数据
       this.$emit('addToDo',todoOBJ)
       e.target.value=''//清空输入框
      },
      
    }
}
</script>

<style>

/*header*/
  .todo-header input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
  }

  .todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
  }
</style>