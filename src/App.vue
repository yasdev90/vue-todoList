<template>
  <div id="app">
    <Header @add-todo="addTodo"/>
    <TodosList :todos="todosList" />
  </div>
</template>

<script>
import Header from './components/header.vue'
import TodosList from './components/todos-list.vue'

export default {
  name: 'App',
  components: {
    Header,
    TodosList
  },
  data(){
    return{
      todosList:[
        {
          id:1,
          title:'develop',
          completed: false,
        },
        {
          id:2,
          title:'test',
          completed: true,
        },
        {
          id:3,
          title:'deploy',
          completed: false,
        },
      ]
    }
  },
  methods:{
    addTodo(newTodo){
      this.todosList.push(newTodo)
    }
  }
}
</script>

<style>
body{
  margin:0;
}

</style>
