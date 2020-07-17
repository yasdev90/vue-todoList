<template>
    <div class='items-container'>
        <div v-bind:key="todo.id" v-for="todo in todos">
            <TodoItem  :todo = "todo" v-on:del-todo="$emit('del-todo', todo.id)"/>
        </div>        
    </div>
</template>
<script>
import TodoItem from './todo-item.vue'
export default {
    name:'TodosList',
    props:["todos"],
     components:{
         TodoItem,
     }
}
</script>
<style>
.items-container{
    
    padding: 50px 12px;
}

.btn-navigate{
    padding: 12px;
    background-color: #333333;
    color:#ffffff;
    border:2px solid #333333;
    border-radius: 12px;
    cursor: pointer;
margin :0 8px;
}
.btn-navigate:hover{
    padding: 12px;
    background-color: #ffffff;
    color:#333333;
    border:2px solid #333333;
    border-radius: 12px;
    cursor: pointer;
margin :0 8px;
}
.btn-navigate[disabled]{
  cursor: not-allowed;
  opacity: 0.5;
}
.btn-navigate[disabled]:hover{
  cursor: not-allowed;
  opacity: 0.5;
   background-color: #333333;
    color:#ffffff;
    border:2px solid #333333;
}

select{
    padding : 12px;
    margin: 0 8px;
    border-radius: 12px;
    border: 2px solid #333333;
}

.devider{
    border-left: 1px solid #000000;
    margin:0 12px 0 4px;
}
</style>