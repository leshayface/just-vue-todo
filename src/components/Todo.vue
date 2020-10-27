<template>
  <div>
    <input type="checkbox" v-model="completed" @change="doneEditing" /> <!-- bind completed property to checkbox -->
    <p class="text-red-500" v-if="!editing" @dblclick="editTodo(todo)">{{title}}</p> <!-- if editing isnt true, display the todo item -->
    <!-- if editing is true, display the editing input. End editing on enter / blur. On escape key call cancelEditing method -->
    <input
      v-else type="text"
      v-model="title"
      @blur="doneEditing(todo)"
      @keyup.enter="doneEditing(todo)"
      @keyup.esc="cancelEditing(todo)"
      v-focus
    />
    <h3 @click="removeTodo(index)">X</h3> <!-- on click call removeTodo method -->
  </div>
</template>

<script>
  export default {
    name: 'todo',
    props: {
      todo: {
        type: Object,
        required: true,
      },
      index: {
        type: Number,
        required: true,
      }
    },
    data() {
      return {
        'id': this.todo.id,
        'title': this.todo.title,
        'completed': this.todo.completed,
        'editing': this.todo.editing,
        'beforeEditCache': '',
      }
    },
    directives: {
      focus: {
        inserted: function (el) {
          el.focus()
        }
      }
    },
    methods: {
      removeTodo(index) {
        this.$emit('removedTodo', index)
      },
      editTodo() {
        this.beforeEditCache = this.title;
        this.editing = true;
      },
      doneEditing() {
        //if edited title is empty set it to the previous text
        if (this.title.trim() == '') {
          this.title = this.beforeEditCache
        }
        this.editing = false
        this.$emit('finishedEdit', {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editing': this.editing,
        })
      },
      cancelEditing() {
        //set editing to false and set text back to its previous string
        this.editing = false;
        this.title = this.beforeEditCache
      }
    }
  }
</script>