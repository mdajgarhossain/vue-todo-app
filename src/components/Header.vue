<template>
  <header class="header">
    <h1>todos</h1>
    <input
      class="new-todo"
      placeholder="What needs to be done?"
      @blur="addTodo"
      @keyup.enter="addTodo"
      v-model="newTodo"
      autofocus
    />
    <p v-show="!error">{{ newTodo }}</p>
    <p v-show="error" class="warning">This item is already exist</p>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    STORAGE_KEY: {
      type: String,
      default: "",
    },
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
        // id: this.todos.length,
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
      let todos = JSON.parse(localStorage.getItem(this.STORAGE_KEY) || "[]");
      todos.push(this.saveablData);
      localStorage.setItem(this.STORAGE_KEY, JSON.stringify(todos));
      //this.todos = todos;
      this.newTodo = "";
      this.$emit("set-todos", todos);
    },
  },
};
</script>

<style>
</style>