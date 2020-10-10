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
          <li v-for="todo in todos" v-bind:key="todo.id" class="todo">
            <div class="view">
              <input class="toggle" type="checkbox" v-model="todo.completed" />
              <label>{{ todo.title }}</label>
              <button class="destroy" v-on:click="removeTodo(todo)"></button>
            </div>
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
export default {
  name: "App",
  data() {
    return {
      newTodo: "",
      todos: [{ id: 0, title: "My todo", completed: false }],
    };
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
    },
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    },
  },
};
</script>

<style>
@import "../src/assets/app.css";
</style>
