<template>
  <header class="header">
    <input
      class="new-todo p-3 border bg-light"
      placeholder="What needs to be done?"
      @blur="addTodo"
      @keyup.enter="addTodo"
      v-model="newTodo"
      autofocus
    />
    <div v-show="error" class="alert alert-warning text-danger" role="alert">
      This item is already exist!
    </div>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    todos: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      newTodo: "",
      error: false,
    };
  },
  computed: {
    saveablData() {
      return {
        // id: this.todos.length + 1,
        title: this.newTodo,
        completed: false,
      };
    },
  },
  watch: {
    newTodo: {
      deep: true,
      handler(n, o) {
        if (this.error && o != n) {
          this.error = false;
        }
      },
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo) {
        if (this.todos.length) {
          const sameTodo = this.todos.find(
            (todo) => todo.title.toUpperCase() == this.newTodo.toUpperCase()
          );
          if (sameTodo) {
            this.error = true;
            return;
          }
        }
        this.saveTodoItems();
        //this.todos.push(this.saveablData);
      }
    },
    saveTodoItems() {
      let todos = JSON.parse(localStorage.getItem(this.$STORAGE_KEY) || "[]");
      todos.unshift(this.saveablData);
      localStorage.setItem(this.$STORAGE_KEY, JSON.stringify(todos));
      //this.todos = todos;
      this.newTodo = "";
      this.$emit("save-todos", todos);
    },
  },
};
</script>

<style scoped>
.new-todo {
  width: 400px;
  font-size: 25px;
}
.header input::-webkit-input-placeholder {
  color: #c5bcbc;
  font-style: italic;
}
</style>