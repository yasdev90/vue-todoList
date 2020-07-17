<template>
    <div class="controls-container">
        <button class="btn-navigate" :disabled="this.allIsActive" @click="showAll">Show all {{this.todos.length}} todos</button>
       , having 
       <button class="btn-navigate" :disabled="this.activeIsActive" @click="showActive">{{this.todos.filter(todo=>!todo.completed).length}} active </button> 
       todo(s) and 
       <button class="btn-navigate" :disabled="this.completedIsActive" @click="showcompleted">{{this.todos.filter(todo=>todo.completed).length}} completed </button> one(s)
    </div>
</template>
<script>
export default {
    name: 'TodosInfo',
    props:["todos"],
    data(){
        return{
            allIsActive:false,
            activeIsActive:false,
            completedIsActive:false,
        }
    },
 methods:{
showAll(){
    this.allIsActive=true
    this.activeIsActive=false
    this.completedIsActive=false
    this.$emit('show-all')
},
showActive(){
   this.allIsActive=false
    this.activeIsActive=true
    this.completedIsActive=false
    this.$emit('show-active')
},
showcompleted(){
    this.allIsActive=false
    this.activeIsActive=false
    this.completedIsActive=true
    this.$emit('show-completed')
}
 },
 created(){
     this.allIsActive=true
 }
}
</script>
<style scoped>
</style>