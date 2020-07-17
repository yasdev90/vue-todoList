<template>
    <div class="controls-container">   
        Show     
       <select v-model="filterOption" @change="$emit('filter-changed', filterOption)">
           <option v-for="option in showOptions" :key="option.value" :value="option.value">{{option.text}}
           </option>
       </select>
       Todo(s)
    </div>
</template>
<script>
export default {
    name: 'TodosInfo',
    props:["showOptions"],
    
 created(){
     this.filterOption = 'all'
     console.log(this.showOptions)
 },
}
</script>
<style scoped>
</style>