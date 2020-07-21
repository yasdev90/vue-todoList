<template>
    <div>
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
        </div>
</template>
<script>
export default {
    name:"PagesNavigation",
    props:["todos", "currentPage", "totalPages", ],
    data(){
        return{
selectedPageSize : localStorage.itemsPerPage
        }
    }
}
</script>