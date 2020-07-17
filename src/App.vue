<template>
  <div id="app">
    <Header @add-todo="addTodo"/>
    <div class="container">
    <div class="right">
      <TodosInfo :todos="todosList" @show-active="showActiveTodos" @show-completed="showCompletedTodos" @show-all="showAllTodos"/>
    </div>   
    <div class="left">
      <Paging :currentPage="pageIndex" :totalPages="filterdTodos.length/itemsPerPage" :todos="filterdTodos"  @prev-page="prevPage" @next-page="nextPage" @page-size-changed="setPageSize" />
    </div> 
    <TodosList v-bind:todos="displayedTodos" />       
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/header.vue'
import TodosList from './components/todos-list.vue'
import TodosInfo from './components/todos-info.vue'
import Paging from './components/pages-navigation.vue'

export default {
  name: 'App',
  components: {
    Header,
    TodosList,
    Paging,
    TodosInfo,
  },
  props:["pageSize", "todosList"],
  data(){
    return{
      displayedTodos:[],
      filterdTodos:[],
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
      this.displayedTodos = this.filterdTodos.slice(this.lastItemIndex, parseInt(this.lastItemIndex) + parseInt(this.itemsPerPage))
    },
    setPageSize: function(pageSize){
     this.itemsPerPage = pageSize
     this.lastItemIndex = 0
     this.pageIndex = 1
     this.displayTodos()
    },
    showActiveTodos(){
      this.filterdTodos = this.todosList.filter(todo=> !todo.completed)
      this.displayTodos()
    },
    showCompletedTodos(){
      this.filterdTodos = this.todosList.filter(todo=> todo.completed)
      this.displayTodos()
    },
    showAllTodos(){
      this.filterdTodos = this.todosList
      this.displayTodos()
    },
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos').then(res=>{
      this.todosList =  this.filterdTodos = res.data
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

.right{
  float: right;
}
.left{
  float: left;
}
.container{
  padding: 0 12px;
}
</style>
