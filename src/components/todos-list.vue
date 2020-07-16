<template>
    <div class='items-container'>
        <div class="controls-container">
            <button @click="$emit('prev-page')" class="btn-navigate" :disabled="currentPage == 1">Previous</button>
            page {{currentPage}} of {{totalPages}} page(s)
            <button @click="$emit('next-page')" class="btn-navigate" :disabled="currentPage ==  totalPages">Next</button>
            <span class='devider'></span>
            <span>show</span>
            <select v-model="selectedPageSize" @change="$emit('page-size-changed', selectedPageSize)">
                <option value="5"> 5 items</option>
                <option value="10"> 10 items</option>
                <option value="20"> 20 items </option>
                <option value="50"> 50 items</option>
            </select>
            <span> per page</span>
        </div>        
        <div v-bind:key="todo.id" v-for="todo in todos">
            <TodoItem  v-bind:todo = "todo" v-on:del-todo="$emit('del-todo', todo.id)"/>
        </div>
        
    </div>
</template>
<script>
import TodoItem from './todo-item.vue'
export default {
    name: 'Todos', 
    props:["todos", "currentPage", "totalPages"],
    components:{
        TodoItem,
    },
    data(){
        return{
            selectedPageSize :10
        }
    },
}
</script>
<style scoped>
.items-container{
    padding: 12px;
}
.controls-container{
    text-align: center;
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
.btn-navigate[disabled]{
  cursor: not-allowed;
  opacity: 0.5;
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