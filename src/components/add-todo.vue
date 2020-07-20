<template>
    <form @submit="addTodo">
        <button @click="showControls" :style="{display:[this.addControlsVisible?'none':'block']}">Add new todo...</button>
        <div class="controls-container" :style="{display:[this.addControlsVisible?'block':'none']}">
        <input v-model='title' type="text" name='title' placeHolder="type in title...">
        <input type="submit" value="Add">
        </div>
    </form>
</template>
<script>
export default {    
    name: 'AddTodo', 
    methods:{ 
        addTodo(e){
            e.preventDefault ();
            if(this.title!=''){
            const newTodo = {                
                title: this.title,
                complete : false,
            }
            this.$emit('add-todo', newTodo);
            this.title = '';
             this.addControlsVisible = false;
            }
        }, 
        showControls(){
            this.addControlsVisible = true;
        }        
    },
    data(){
        return{
            title:'',
            addControlsVisible :false,
        }
    },    
}

</script>
<style scoped>
form{
    text-align: center;
    background-color: #333333;
    padding-bottom: 24px;
}
    input[type="text"] {
   margin:0 12px;
    padding: 12px 20px;
    font-size: 16px;
    line-height: 12px;
    }
    input[type="submit"], button {
    padding: 12px 24px;
    line-height: 16px;
    background-color: #333333;
    color: #ffffff;
    font-size: 16px;
    outline: none;
    border: 2px solid #ffffff;
    border-radius: 12px;
    }
    button{
        margin:auto;
    }
</style>