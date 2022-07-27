<template>
    <ul class="todo-main">
      <!--传出数据-->
      <Item v-for="todoOBJ in todos" :key="todoOBJ.id" :todo="todoOBJ" :changeToDo="changeToDo"/>
    </ul>
</template>

<script>

import Item from './Item'
export default {
    name:'List',
    components:{
        Item
    },
    props:['todos','changeToDo']

}
</script>

<style>
  /*main*/
  .todo-main {
    margin-left: 0px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding: 0px;
  }

  .todo-empty {
    height: 40px;
    line-height: 40px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding-left: 5px;
    margin-top: 10px;
  }
</style>