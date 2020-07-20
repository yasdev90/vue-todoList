<template>
  <div id="app">
    <Header />
    <AddTodo @add-todo="addTodo"/>
    <div class="container">
    <div class="right">
      <TodosInfo :showOptions="showOptions" @filter-changed="applyFilter"/>
    </div>   
    <div class="left">
      <Paging :currentPage="pageIndex" :totalPages="Math.ceil(filterdTodos.length/itemsPerPage)" :todos="filterdTodos"  @prev-page="prevPage" @next-page="nextPage" @page-size-changed="setPageSize" />
    </div> 
    <TodosList :todos="displayedTodos" />       
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/header.vue'
import TodosList from './components/todos-list.vue'
import TodosInfo from './components/todos-info.vue'
import Paging from './components/pages-navigation.vue'
import AddTodo from './components/add-todo'

export default {
  name: 'App',
  components: {
    Header,
    TodosList,
    Paging,
    TodosInfo,
    AddTodo
  },
  props:["pageSize"],
  data(){
    return{
      displayedTodos:[],
      filterdTodos:[],
      pageIndex : 1,
      itemsPerPage:10,
      lastItemIndex :0,
     showOptions:[],
     todosListData:[]
    }
  },
  methods:{
    addTodo(newTodo){
      this.todosListData.push(newTodo)
      //this.filterdTodos.push(newTodo)
      this.displayTodos()
    },
    nextPage:function (){    
      if(this.pageIndex<this.todosListData.length/this.itemsPerPage) {
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

    applyFilter(filterValue){
      if(filterValue.toUpperCase()== 'ALL'){
      this.filterdTodos = this.todosListData
      }
      else if(filterValue.toUpperCase()== 'A'){
      this.filterdTodos = this.todosListData.filter(todo=> !todo.completed)
      }
       else if(filterValue.toUpperCase()== 'C'){
     this.filterdTodos = this.todosListData.filter(todo=> todo.completed)
      }
     this.lastItemIndex = 0
     this.pageIndex =1
      this.displayTodos()
    },
    getShowOptions(){
      return this.showOptions = [
         {
             text:"all",
             value : 'all'
         },
         {
             text:"active",
             value : 'a'
         },
          {
             text:"completed",
             value :'c'
         },
     ]
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos').then(res=>{
      this.todosListData =  this.filterdTodos = res.data
      this.displayTodos()
      this.getShowOptions()
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
  padding: 24px 12px;
}
</style>
