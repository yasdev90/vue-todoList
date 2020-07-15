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
export default {
    name:'TodoItem',
  props:["todo"],
  methods:{
      markComplete(){
          this.todo.completed= !this.todo.completed;
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