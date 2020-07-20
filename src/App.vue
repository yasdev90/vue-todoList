<template>
  <div id="app">   
    <Header /> 
    <router-view/>
  </div>
</template>

<script>
import Header from './components/header'
export default {
  name: 'App',
  components: {
    Header
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #8f8e8e;
  text-decoration: none;
  font-size: 24px;
}
#nav a.router-link-exact-active {
  color: #42b983;
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
