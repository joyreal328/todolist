<template>


  <div class="wrapper wrapper-mobile">

      <h1 class="title title-mobile">Simple To do List</h1>

      <div class="form-container form-container-mobile">
                <input type="text" class="todo-input" placeholder="What's your focus for today?" v-model="newTodo" @keyup.enter="addTodo">
                    
                <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">

                <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">

                <div class="todo-item-left">
                    <input type="checkbox" v-model="todo.completed">
                    
                    <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>

                    <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>

                </div>

              
                <div class="remove-item" @click="removeTodo(index)">
                    &times;
                </div>
                </div>
                </transition-group>

                <div class="footer">
                <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label></div>
                <div>{{ remaining }} items left</div>
                </div>

                <div class="footer">
                <div>
                    <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
                    <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
                    <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
                </div>

                <div>
                    <transition name="fade">
                    <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
                    </transition>
                </div>

        </div>
    </div>

    <div class="date">
      <p class="p-mobile">  {{date}}</p>
    </div>
       <div class="date">
      <p class="p-mobile"><a href="https://www.joyreal.site">www.joyreal.site</a></p>
    </div>

  </div>



</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 1,
      beforeEditCache: '',
      filter: 'all',
      todos: [

      ],
      // date: new Date().toJSON.split('T')[0],
      date: new Date()
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  //directives: copied from the vue js doc
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      })

      this.newTodo = ''
      this.idForTodo++
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title
      todo.editing = true
    },
    doneEdit(todo) {
      if (todo.title.trim() == '') {
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
    },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<style>
  
@import url('https://fonts.googleapis.com/css?family=Quicksand&display=swap');
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
@import url('https://fonts.googleapis.com/css?family=Lily+Script+One&display=swap');

  body {
      font-family: 'Quicksand', sans-serif;
      background-image: url(https://source.unsplash.com/Rl9l9mL6Pvs/1700x1000);
      background-repeat: no-repeat;
      background-size: cover;
  }
  
  a {
    text-decoration: none;
    color: white;
  }
  .wrapper {
      width: 90%;
      padding-top: 15%;
      margin: auto;
      height: 100vh;
  }

  .form-container {
      padding: 2em;
      margin: auto;
      border-radius: 50px;
      background-color: white;
      opacity: .75;
  }

  .title {
      font-family: 'Lily Script One', cursive;
      color: white;
  }

  .todo-input {
    width: 80%;
    padding: 15px;
    font-size: 18px;
    margin-bottom: 25px;
    padding-bottom: 25px;
    margin: auto;
    font-family: 'Quicksand', sans-serif;
    border: none;
  }

  .todo-item {
    margin-bottom: 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
    padding: 5px;

  }

  .remove-item {
    cursor: pointer;
    margin-left: 14px;
   
  }

  .todo-item-left { 
    display: flex;
    align-items: center;
  }

  .todo-item-label {
    padding: 5px;
    border: 1px solid white;
    margin-left: 12px;
  }

  .todo-item-edit {
    font-size: 20px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc; 
  }

  .completed {
    text-decoration: line-through;
    color: red;

  }


  p {
    color: white;
  }

  .footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 20px;
    margin-bottom: 14px;
  }

  button {
    font-family: 'Quicksand', sans-serif;
    font-size: 12pt;
    background-color: white;
    appearance: none;
    color: lightgrey;
 
   
  }

  .active {
    background: #055777;
  }

 
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }

  .fade-enter, .fade-leave-to {
    opacity: 0;
  }


 @media only screen and (max-width: 768px) {
  /* For mobile phones: */
  body {
      font-family: 'Quicksand', sans-serif;
      background: #055777;
  }


  .wrapper-mobile {
      width: 100%;
      padding-top: 15%;
      margin: auto;

  }

   .form-container-mobile {
      padding: .5em;
      margin: auto;
      border-radius: 20px;
      background-color: white;
      opacity: .9;
      width: 90%;
      text-align: center;
  }


    .todo-input-mobile {
    width: 100%;
    padding: 15px;
    font-size: 10px;
    margin-bottom: 20px;
    padding-bottom: 20px;
    margin: auto;
    font-family: 'Quicksand', sans-serif;
    border: none;
    position: relative;
  }


  .title-mobile {
     width: 80%;
     margin: auto;
     text-align: center;
     padding:5%;
  }

  .p-mobile{
    padding:2%;
    font-size: 10pt;

  }



  @media (min-width: 992px)  
  {

    body {
      background: black;
    }

}

}







</style>


<!--
<template>

    <div>
        <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">

        
        <div v-for="(singletodo, index) in todos" :key="singletodo.id"
        class="todo-item">

            <div class="todo-item-left">
                <input type="checkbox" v-model="singletodo.completed" >
                <div v-if="!singletodo.editing" @dblclick="editTodo(singletodo)" class="todo-item-label" :class="{ completed: singletodo.completed }">
                    {{ singletodo.title }}
                </div>
                <input v-else class="todo-item-edit" type="text" v-model="singletodo.title" @blur="doneEdit(singletodo)" @keyup.enter="doneEdit(singletodo)" v-focus >
                
            </div>

            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div>
            

            

        </div>
    </div>

  
    
</template>

<script>
export default {
    name: 'todo-list',
    data () {
        return {
            newTodo: '',
            idForTodo: 1,
            todos: [
                /*
                {
                    'id': 1,
                    'title': 'Take a bath',
                    'completed': false,
                    'editing': false,
                },
                {
                    'id': 2,
                    'title': 'Eat breakfast',
                    'completed': false,
                    'editing': false

                }
                */
            ]
        }
    },

    directives: {
        focus: {
        // directive definition
        inserted: function (el) {
        el.focus()
        }
    }
    },

    methods: {
        addTodo() {

        //validation
        if(this.newTodo.trim().length == 0){
            return
        }
        
        //adding items on the array

            this.todos.push({
                id:this.idForTodo,
                title:this.newTodo,
                completed: false,

            })

            this.newTodo = ''
            this.idForTodo++
        },

        removeTodo(index) {
            this.todos.splice(index, 1)
        },

        editTodo(singletodo) {
            singletodo.editing=true;
        },

        doneEdit(singletodo) {


            singletodo.editing=false; //flip the edit

        }
    }


    
}
</script>

<style scoped>

.todo-input {
    width: 100%;
    padding: 15px;
    font-size: 18px;
    margin-bottom: 1em;
    margin-top: 1em;
}

.todo-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
    align-items: center;
}

.todo-item-left {
    display: flex;
    align-items: center;
}

.remove-item {
    cursor: pointer;
    margin-left: 14px;
  
}



.todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
}

.todo-item-edit {
    font-size: 25px;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    
}

.completed {
    text-decoration: line-through;
    color: grey;
}






</style>

-->