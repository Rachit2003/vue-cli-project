<template>

<body>
    
    <div  class="container">
        <h1>Todo List</h1>
        <h2>New ToDo</h2>
        <div>
            <input type="text" v-model="todo" class="int" >
            <h4 v-if="isError" style="margin-top:0px;padding-left:55px;">Text field required</h4>
        </div>
        <div class="priority">
            <h3 style="color:white;padding-top:11px;margin:0px;padding-left:60px;">Set Priority:</h3>
            <input type="text"  class="int2" @keypress="validateNumber" v-model="priority" /> 
            <h4 v-if="isErrorNum" style="margin-top:10px;">*Please enter priority </h4> 
           
            <!-- <input type="submit" value="Add" @click="storepriority" class="btn-priority"> -->
           
        </div>
        <input type="submit" value="Add" @click="storeTodo" class="btn">

        <h3>To-Do List</h3>
        <hr size="2" width="95%" color="white">
        
    <div >
        <ul>
            <li v-for="(todo, index) in todos"  :key=index>
                <div  :class="{'strikeout': todo.isStrikedoff ==true}">
                {{ todo.seq }} -
                {{ todo.name }}
                 </div>      
             <button @click="deleteTodo(index)" class="edit">Remove</button> 

            </li>
              
        </ul>
          <h3 style="padding-top:80px;padding-left:20px;">Deleted To-Do Items</h3>
          <hr size="2" width="95%" color="white">
      
     
         <!-- <div class="deleted"> -->
       <ul>
          
          <li v-for="(todo, index) in removedTodos" :key="index" >
                       <div class="strikeout">
                          {{ todo.seq }} -
                          {{ todo.name }}
                       </div>  
            <div class="restore">  
             <button @click="restoreTodo(index),clearTodo(index)" class="edit">Restore</button> 
               <button @click="clearTodo(index)" class="edit">Delete</button> 
       
             </div>
          </li>
          
       </ul>
     
         <!-- </div> -->
     
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
                priority: '',
                priorities : [],
                isError: false,
                isErrorNum:false,
                // removeTodo:'',
                removedTodos:[]
              }
            },            
            methods: {
                validateNumber()
                {
                
                let keyCode = event.keyCode;
                if(keyCode < 48 || keyCode > 57)
                {
                    event.preventDefault();
                    this.isErrorNum = true
                }
                else{
                    this.isErrorNum = false
                }
                },
               
                storeTodo() {
                    
                    if(this.todo!=""){
                    this.todos.push(
                   { name:this.todo ,   
                    seq:Number(this.priority), isStrikedoff:false})     
                    this.todos.sort( (a, b) => {return a.seq - b.seq } )
         
                    this.isError=false
                    this.priority=''
                    this.todo=''
                    
                    }
                    
                    else{
                        this.isError=true
                    }
                    
                },
                deleteTodo(index) {
                    this.removedTodos.push(...this.todos.splice(index, 1));
                },
                clearTodo(index){
                    this.removedTodos.splice(index,1)
                },
                // movetodo(){
                //         // if(this.removeTodo==true){
                //         this.value.push(
                //     { name:this.todo ,   
                //         seq:Number(this.priority), isStrikedoff:false}) 
                //         }
                //     // }
                
                //     }
                restoreTodo(index){
                     this.todos.push(...this.removedTodos.splice(index, 1))
                      this.todos.sort( (a, b) => {return a.seq - b.seq } )

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
