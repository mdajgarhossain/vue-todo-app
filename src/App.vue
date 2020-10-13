<template>
  <div id="app">
    <section class="todoapp">
      <input type="text" v-model="search" placeholder="Search here" />
      <span>{{ search }}</span>
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
      <section class="main">
        <ul class="todo-list">
          <li
            v-for="todo in filterdTodos"
            v-bind:key="todo.id"
            v-bind:class="{
              completed: todo.completed,
              editing: todo == editedTodo,
            }"
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
      <footer class="footer">
        <ul class="filters">
          <li>
            <a
              href="#"
              v-on:click.prevent="visibility = 'all'"
              :class="{ selected: visibility == 'all' }"
              >All</a
            >
          </li>
          <li>
            <a
              href="#"
              v-on:click.prevent="visibility = 'active'"
              :class="{ selected: visibility == 'active' }"
              >Active</a
            >
          </li>
          <li>
            <a
              href="#"
              v-on:click.prevent="visibility = 'completed'"
              :class="{ selected: visibility == 'completed' }"
              >Completed</a
            >
          </li>
        </ul>
      </footer>
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
      visibility: "all",
      error: false,
      search: "",
    };
  },
  created() {
    this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
  },
  computed: {
    filterdTodos() {
      //all, active, completed
      if (this.visibility === "all") {
        return this.todos;
      } else if (this.visibility === "active") {
        return this.todos.filter((todo) => !todo.completed);
      } else {
        return this.todos.filter((todo) => todo.completed);
      }
    },
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
    search: {
      deep: true,
      handler(n,o) {
        if (o != n) {
          this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
        }
        this.handleSearch();
      },
    },
  },
  methods: {
    handleSearch() {
      this.todos = this.todos.filter((todo) => {
        console.log(todo);
        return todo.title.match(this.search);
      });
    },
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
        this.todos.push(this.saveablData);
      }
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
