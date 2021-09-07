<template>

<body>
    
    <div  class="container">
        <h1>Todo List</h1>
        <h2>New ToDo</h2>
        <div>
            <input type="text" v-model="todo" class="int" >
            <h4 v-if="isError" style="margin-top:0px;padding-left:25px;margin-bottom:0px;">*Text field required</h4>
        </div>
        <div class="priority">
            <input type="text" placeholder="Set Priority" class="int2" @keypress="validateNumber" v-model="priority" /> 
                   
        </div>
           <h4 v-if="isErrorNum" style="margin-top:0px;padding-left:25px;margin-bottom:0px;">*Please enter priority </h4>
            <input type="text" v-model="description" placeholder="Description" class="int3"> 
            <input type="text" v-model="category " placeholder="Category" class="int3"> 
            <input type="submit" value="Add" @click="trial1();storeTodo()" class="btn">
    <div style="display:flex;justify-content:space-between;width:90%;height:55px">
        <div style="width:120px;"> <h3 >To-Do List</h3></div>
         <div style="margin-top:20px;">

         <select class="form-control" v-model="selectedCategory">       
         <option value="" selected disabled >Category</option>              
         <option v-for="(todo,index) in todos"  :key="index" style="background-color:white;color:black;">{{ todo.catg }}</option>
         </select>
        
        </div>
        </div>
        <hr size="2" width="95%" color="white">
        
    <div >
        <ul>
            <li v-for="(todo, index) in computed_items"  :key=index>
                <div  class="{'strikeout': todo.isStrikedoff ==true} ">
                   
                {{ todo.seq }} -
                {{ todo.name }}
                <br>
               
                <div class="descp">           
                Description- {{todo.desc}}
                </div>
                </div>      
                 
                <button @click="deleteTodo(index)" class="edit">Remove</button> 

            </li>
              
        </ul>
          <h3 style="padding-top:80px;padding-left:20px;">Deleted To-Do Items</h3>
          <hr size="2" width="95%" color="white">
      
     
        
       <ul>
          
        <li v-for="(todo, index) in removedTodos" :key="index" >
            <div class="strikeout">
                          {{ todo.seq }} -
                          {{ todo.name }}
                           <br>
                <div class="descp">           
                 Description- {{todo.desc}}
                </div>
            </div>  
                      
            <div class="restore">  
             <button @click="restoreTodo(index)" class="edit">Restore</button> 
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
                description: '',
                category:'',
                selectedTodo: null,
                priority: '',
                priorities : [],
                isError: false,
                isErrorNum:false,
                // removeTodo:'',
                removedTodos:[],
                trial:[],
                store:false,
                selectedCategory: 0
              }
            },   
            computed: {
            computed_items: function () {
                let filtertype = this.todos.filter((a)=>{
                if(a.catg === this.selectedCategory){
                    return a;
                }
                })
                return filtertype;
    //         let filterType= this.selectedCategory
    //         return this.todos.filter(function(todo){
    //           let filtered = true
    //         if(filterType && filterType.length > 0){
    //            filtered = todo.catg == filterType
    //         }
           
    //     return filtered
    //   })
    }
  
             },
            mounted() 
                {
                    console.log('App mounted!');
                    if (localStorage.getItem('todos')) this.todos = JSON.parse(localStorage.getItem('todos'));
                    if (localStorage.getItem('removedTodos')) this.removedTodos = JSON.parse(localStorage.getItem('removedTodos'));
                },
                 watch: 
                {
                todos: 
                    {
                    handler()                         {
                    console.log('Todos changed!');
                    localStorage.setItem('todos', JSON.stringify(this.todos));
                    },
                     deep: true,
                    },
                    removedTodos: 
                    {
                    handler() 
                    {
                        console.log('Todos changed!');
                        localStorage.setItem('removedTodos', JSON.stringify(this.removedTodos));
                    },
                     deep: true,
                     },
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
                    seq:Number(this.priority),desc:this.description,catg:this.category, isStrikedoff:false})     
                    this.todos.sort( (a, b) => {return a.seq - b.seq } )
                    this.isError=false                 
                    this.priority=''
                    this.todo=''
                    this.description=''
                    this.category=''                  
                    }                   
                    else
                    {
                        this.isError=true
                    }
                  
                },
               
                deleteTodo(index) {
                    this.removedTodos.push(...this.todos.splice(index, 1));
                  

                },
                clearTodo(index){
                    this.removedTodos.splice(index,1)
                    this.trial.splice(index,1)
                  
                
                },
              
                restoreTodo(index){
                    this.todos.push(...this.removedTodos.splice(index, 1))
                    this.todos.sort( (a, b) => {return a.seq - b.seq } )
                   


                },
                trial1(){
                    this.trial.push({cat:this.category})
                  
                },
                display(){
                    if(this.Option.todo.catg==this.todo.catg){
                        console.log('Display called')
                    }
            
                }
              
                

                
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
