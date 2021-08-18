<template>
 
<body>
    <div  class="container">
        <h1>Todo List</h1>
        <h2>New ToDo</h2>
        <div>
            <input type="text" v-model="todo" class="int" >
            <h4 v-if="isError">Text field required</h4>
            <input type="submit" value="Add" @click="storeTodo" class="btn">
        </div>
        
        <h3>To-Do List</h3>
        <hr size="1" width="95%" color="white">
<div >
        <ul>
            <li v-for="(todo, index) in todos" :key=index>
                {{ todo }}
                <button @click="removeTodo(index)" class="edit">Delete</button>
            </li>
        </ul>
        
    </div>
    </div>
</body>
</template>

<script>

export default {
  name: 'App',
           
            data() {
              return {
                todo: '',
                todos: [],
                selectedTodo: null,
                isError: false
               
              }
            },            
            methods: {
               
                storeTodo() {
                    
                    if(this.todo!=""){
                    this.todos.push(this.todo)
                    this.todo = ''              
                    this.isError=false
                    
                    }

                    else{
                        this.isError=true
                    }
                    
                },
                removeTodo(index) {
                    this.todos.splice(index, 1)
                },
                
            }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

</style>
