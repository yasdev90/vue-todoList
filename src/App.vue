<template>
  <div id="app">
    <Header @add-todo="addTodo"/>
    <TodosList :todos="displayedTodos" :currentPage="pageIndex" :totalPages="todosList.length/itemsPerPage" @prev-page="prevPage" @next-page="nextPage" @page-size-changed="setPageSize" />
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/header.vue'
import TodosList from './components/todos-list.vue'

export default {
  name: 'App',
  components: {
    Header,
    TodosList
  },
  props:["pageSize"],
  data(){
    return{
      todosList:[],
      displayedTodos:[],
      pageIndex : 1,
      itemsPerPage:10,
      lastItemIndex :0,
    }
  },
  methods:{
    addTodo(newTodo){
      this.todosList.push(newTodo)
    },
    nextPage:function (){    
      if(this.pageIndex<this.todosList.length/this.itemsPerPage) {
      this.pageIndex ++;
      this.lastItemIndex += parseInt(this.itemsPerPage)
      this.displayTodos()
      }
    },
    prevPage:function (){
      if(this.pageIndex>1){
      this.pageIndex --;
      this.lastItemIndex -= parseInt(this.itemsPerPage)
       this.displayTodos()
      }
    }, 
    displayTodos: function(){
      this.displayedTodos = this.todosList.slice(this.lastItemIndex, parseInt(this.lastItemIndex) + parseInt(this.itemsPerPage))
    },
    setPageSize: function(pageSize){
     this.itemsPerPage = pageSize
     this.lastItemIndex = 0
     this.pageIndex = 1
     this.displayTodos()
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos').then(res=>{
      this.todosList = res.data
      this.displayTodos()
      })   
  }  
}
</script>

<style>
body{
  margin:0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

</style>
