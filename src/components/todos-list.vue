<template>
    <div class='items-container'>
        <div v-bind:key="todo.id" v-for="todo in todos">
            <TodoItem  v-bind:todo = "todo" v-on:del-todo="$emit('del-todo', todo.id)"/>
        </div>
    </div>
</template>
<script>
import TodoItem from './todo-item.vue'
export default {
    name: 'Todos', 
    props:["todos"],
    components:{
        TodoItem,
    }
}
</script>
<style scoped>
.items-container{
    padding: 12px;
}
</style>