<template>
  <div>
    <input type="text" placeholder="What do you need to do mannn?" v-model="newTodo" @keyup.enter="addTodo" /> <!-- use v-model to bind data property -->
    <p>TODO LIST</p>
    <div v-for="(todo, index) in todos" :key="todo.id"> <!-- grab each item from array as well as its index -->
        <input type="checkbox" v-model="todo.completed" /> <!-- bind completed property to checkbox -->
        <p v-if="!todo.editing" @dblclick="editTodo(todo)">{{todo.title}}</p> <!-- if editing isnt true, display the todo item -->
        <!-- if editing is true, display the editing input. End editing on enter / blur. On escape key call cancelEditing method -->
        <input
          v-else type="text"
          v-model="todo.title"
          @blur="doneEditing(todo)"
          @keyup.enter="doneEditing(todo)"
          @keyup.esc="cancelEditing(todo)"
          v-focus
        />
        <h3 @click="removeTodo(index)">X</h3> <!-- on click call removeTodo method -->
    </div>
    <div>{{ remaining }} items left</div> <!-- computed properties can be referred to like data properties -->
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      newTodoID: 3,
      beforeEditCache: '',
      todos: [
        {
          'id': 1,
          'title': 'Finish Building This App',
          'completed': false,
          'editing': false
        },
        {
          'id': 2,
          'title': 'Take Over The World!',
          'completed': false,
          'editing': false
        },
      ]
    }
  },
  methods: {
    addTodo() {
      //use this keyword to access data

      //make sure no empty todo can be added
      if (this.newTodo.trim() == '') {
        return
      }

      this.todos.push(
        {
          id: this.newTodoID, //sets the new todo id to 3
          title: this.newTodo, //grab text from input (v-model) - two way data binding
          completed: false,
          editing: false
        }
      )

      this.newTodo = ''; //set newTodo text to empty
      this.newTodoID = this.newTodoID++; //increment newTodoID for next todo
    },
    removeTodo(index) {
      this.todos.splice(index, 1); //splice todo array by index and remove 1 item
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    doneEditing(todo) {
      todo.editing = false;
      //if edited todo is empty set it to the previous text
      if (todo.title.trim() == '') {
        todo.title = this.beforeEditCache
      }
    },
    cancelEditing(todo) {
      //set editing to false and set text back to its previous string
      todo.editing = false;
      todo.title = this.beforeEditCache
    }
  },
  //computed property is for composing new data derived from other data
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
