<template>
  <div id="app">    
    <AddTodo @add-todo="addTodo"/>
    <div class="container">    
      <div class="right">
        <TodosInfo :showOptions="showOptions" @filter-changed="applyFilter"/>
      </div>   
      <div :class="[pagesCount==0 ?'hidden':'visible']">
      <div class="left">
        <Paging :currentPage="pageIndex" :totalPages="pagesCount" :todos="filterdTodos"  @prev-page="prevPage" @next-page="nextPage" @page-size-changed="setPageSize" />
      </div> 
      </div>
      <TodosList :todos="displayedTodos" @del-todo="deleteTodo" @complete="applyFilter(localStorage.filterValue)"/>
       <div :class="[filterdTodos.length>0 ?'hidden':'visible']" class="alert alert-primary">
      <p>horaay! no tasks you got!</p>  
      </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
import TodosList from '../components/todos-list.vue'
import TodosInfo from '../components/todos-info.vue'
import Paging from '../components/pages-navigation.vue'
import AddTodo from '../components/add-todo'
import staticJson from '../../db.json'
import {appConfigurations} from '../config'
const baseApiUrl = appConfigurations.baseApiUrl

export default {
  name: 'Home',
  components: {
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
      lastItemIndex :0,
      pagesCount:0,
     showOptions:[],
     todosListData:[]
    }
  },
  methods:{
    addTodo(newTodo){
      try{            
        axios.post(baseApiUrl, newTodo).then(res=>{
        this.filterdTodos.push(newTodo)
        this.todosListData =  [...this.todosListData, res.data]
        this.setPageSize(localStorage.itemsPerPage)
        this.displayTodos()
      })
      }catch(e){
        console.log(e)
      }
    },
    nextPage:function (){    
      if(this.pageIndex<this.todosListData.length/localStorage.itemsPerPage) {
      this.pageIndex ++;
      this.lastItemIndex += parseInt(localStorage.itemsPerPage)
      this.displayTodos()
      }
    },
    prevPage:function (){
      if(this.pageIndex>1){
      this.pageIndex --;
      this.lastItemIndex -= parseInt(localStorage.itemsPerPage)
       this.displayTodos()
      }
    }, 
    displayTodos: function(){
      this.displayedTodos = this.filterdTodos.slice(this.lastItemIndex, parseInt(this.lastItemIndex) + parseInt(localStorage.itemsPerPage))
    },
    setPageSize: function(pageSize){
      try{
     localStorage.itemsPerPage = pageSize
     this.lastItemIndex = 0
     this.pageIndex = 1 
     this.pagesCount = Math.ceil(this.todosListData.length/localStorage.itemsPerPage)
     this.displayTodos()
      }
      catch(e){
        console.log(e)
      }
    },

    applyFilter(filterValue){   
      try{ 
      localStorage.filterValue = filterValue
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
     this.pagesCount = Math.ceil(this.filterdTodos.length/localStorage.itemsPerPage)
      this.displayTodos()
      }
      catch(e){
        console.log(e)
      }
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
    },
    deleteTodo(id){
         try{                 
      axios.delete(baseApiUrl+'/'+id).then(()=>{        
        this.todosListData =  this.todosListData .filter(todo=> todo.id !== id)
         this.filterdTodos =  this.todosListData .filter(todo=>todo.id !== id)
        this.displayTodos()
      })}
      catch(e){
        console.log(e)
      }
    }
  },
  beforeCreate(){    
    try{
    axios.get(baseApiUrl).then(res=>{
      if(localStorage.filterValue == undefined) {
          localStorage.filterValue='all'
        }
      if(localStorage.filterValue.toUpperCase()=='ALL'){        
      this.todosListData =  this.filterdTodos = res.data
      }
      else
      {
        this.todosListData =  res.data
        this.applyFilter(localStorage.filterValue)
      }
      if(!localStorage.itemsPerPage){
        localStorage.itemsPerPage = 10
      }
      this.pagesCount = Math.ceil(this.todosListData.length/localStorage.itemsPerPage)
      this.displayedTodos = this.todosListData
      this.displayTodos()
      this.getShowOptions()
      })      
      
      
      .catch(error=>{
        if (!error.response) {
           console.log ('Error: Network Error');
           this.todosListData = this.filterdTodos =  staticJson.todos
           this.displayTodos()
        } else {
            this.errorStatus = error.response.data.message;
        }
      })
      }
      catch(e){
        console.log(e)
      }
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
.visible{
  display: block;
}
.hidden{
  display: none;
}
input[type="checkbox"]{
  width:32px;
  height: 32px;
  margin:12px;
}
</style>
