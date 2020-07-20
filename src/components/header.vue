<template>
    <header class="header">
        <h1>Todo list</h1>
       <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    </header>
</template>
<script>
export default {
    name:'Header',    
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