<template>
    <div class='item' :class="{'completed' : todo.completed}">
        <p>
            <input type="checkbox" :checked="todo.completed" @change="markComplete"/>            
            {{todo.title}}
        </p>
        <button class='delete' @click="$emit('del-todo', todo.id)">Del</button>

    </div>
</template>

<script>
import axios from 'axios'
import {appConfigurations} from '../config'
export default {
    name:'TodoItem',
  props:["todo"],
  data(){
      return{
      baseApiUrl: appConfigurations.baseApiUrl,
      }
  },
  methods:{
     async markComplete(){
          this.todo.completed = !this.todo.completed
         await axios.put(this.baseApiUrl+'/'+this.todo.id, this.todo)
         this.$emit('complete')
      }
  }
}
</script>
<style scoped>
p{
    color:#333333;
    display: inline;
    font-size: 24px;
    
}
.completed p{
    text-decoration: line-through;
    
}

.item{
 padding:32px 0;
position: relative;
border-bottom: 1px solid #333333;
}

.delete{
padding: 12px;
background-color: #ffffff;
display:inline;
position: absolute;
right: 0;
text-align: right;
border:2px solid #70201a;
border-radius: 12px;
color: #70201a;;
margin-block: 4px;;
}
</style>