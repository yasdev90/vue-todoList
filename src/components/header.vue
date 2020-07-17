<template>
    <header class="header">
        <h1>Todo list</h1>
        <AddTodo/>
    </header>
</template>
<script>
import AddTodo from './add-todo.vue'
export default {
    name:'Header',
    components:{
        AddTodo,
    },
}
</script>

<style scoped>
.header{
    font-size: 36px;
    color:#ffffff;
    text-align: center;
    text-transform: capitalize;
    background-color: #333333;
    padding:24px;
}
.header h1 {
    margin: 0 auto 24px auto;
}
</style>