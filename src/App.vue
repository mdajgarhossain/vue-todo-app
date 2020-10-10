<template>
  <div id="app">
    <section class="todoapp">
      <header class="header">
        <h1>todos</h1>
        <input
          class="new-todo"
          placeholder="What needs to be done?"
          v-on:keyup.enter="addTodo"
          v-model="newTodo"
          autofocus
        />
      </header>
      <section class="main">
        <ul class="todo-list">
          <li
            v-for="todo in todos"
            v-bind:key="todo.id"
            v-bind:class="{ editing: todo == editedTodo }"
          >
            <div class="view">
              <input class="toggle" type="checkbox" v-model="todo.completed" />
              <label v-on:dblclick="editTodo(todo)">{{ todo.title }}</label>
              <button class="destroy" v-on:click="removeTodo(todo)"></button>
            </div>
            <input
              class="edit"
              type="text"
              v-model="todo.title"
              v-on:blur="doneEdit(todo)"
              v-on:keyup.enter="doneEdit(todo)"
            />
          </li>
        </ul>
        <input class="toggle-all" id="toggle-all" type="checkbox" />
        <label for="toggle-all">Mark all as complete</label>
        <ul class="todo-list"></ul>
      </section>
      <footer class="footer"></footer>
    </section>
  </div>
</template>

<script>
const STORAGE_KEY = "todo-storage";
export default {
  name: "App",
  data() {
    return {
      newTodo: "",
      todos: [],
      editedTodo: null,
    };
  },
  created() {
    this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
  },
  methods: {
    addTodo() {
      console.log(this.newTodo, this.todos);
      this.todos.push({
        id: this.todos.length,
        title: this.newTodo,
        completed: false,
      });
      this.newTodo = "";
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
    },
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
    },
    editTodo(todo) {
      this.editedTodo = todo;
    },
    doneEdit(todo) {
      if (!this.editedTodo) {
        return;
      }
      this.editedTodo = null;
      todo.title = todo.title.trim();
      if (!todo.title) {
        this.removeTodo(todo);
      }
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
    },
  },
};
</script>

<style>
@import "../src/assets/app.css";
</style>
