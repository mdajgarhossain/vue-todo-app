<template>
  <div id="app">
    <section class="todoapp">
      <!-- <input type="text" v-model="search" placeholder="Search here" />
      <span>{{ search }}</span> -->

      <Header
        @set-todos="setUpatedTodos"
        :STORAGE_KEY="STORAGE_KEY"
        :todos="todos"
      />

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
import Header from "./components/Header";

// const STORAGE_KEY = "todo-storage";
export default {
  name: "App",
  components: {
    Header,
  },
  data() {
    return {
      STORAGE_KEY: "todo-storage",
      todos: [],
      editedTodo: null,
      visibility: "all",
      search: "",
    };
  },
  created() {
    this.todos = JSON.parse(localStorage.getItem(this.STORAGE_KEY) || "[]");
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
  },
  watch: {
    search: {
      deep: true,
      handler(n, o) {
        if (o != n) {
          //this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
          this.handleSearch();
        }
        // this.handleSearch();
      },
    },
    // todos: {
    //   deep: true,
    //   handler(n, o) {
    //     // console.log(newValue);
    //     if(o != n) {
    //       this.saveTodoItems();
    //     }

    //   },
    // },
  },
  methods: {
    setUpatedTodos(todos) {
      this.todos = [...todos];
    },
    handleSearch() {
      let todos = JSON.parse(localStorage.getItem(this.STORAGE_KEY));
      if (todos && todos.length) {
        this.todos = todos.filter((todo) => {
          return todo.title.match(this.search);
        });
      }
    },

    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
      localStorage.setItem(this.STORAGE_KEY, JSON.stringify(this.todos));
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
      localStorage.setItem(this.STORAGE_KEY, JSON.stringify(this.todos));
    },
  },
};
</script>

<style>
@import "../src/assets/app.css";
</style>
