<template>
  <div id="app">
    <section class="todoapp">
      <!-- <input type="text" v-model="search" placeholder="Search here" />
      <span>{{ search }}</span> -->

      <Header @save-todos="saveTodos" :todos="todos" />
      <Todos :todos="todos" :filteredTodos="filteredTodos" />
      <Footer @handleClick="checkVisibility" :todos="todos" />
    </section>
  </div>
</template>

<script>
import Header from "./components/Header";
import Todos from "./components/Todos";
import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Header,
    Todos,
    Footer,
  },
  data() {
    return {
      todos: [],
      search: "",
      visibility: "all",
    };
  },
  created() {
    this.todos = JSON.parse(localStorage.getItem(this.$STORAGE_KEY) || "[]");
  },
  computed: {
    filteredTodos() {
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
  },
  methods: {
    saveTodos(todos) {
      this.todos = [...todos];
    },
    checkVisibility(value) {
      this.visibility = value;
    },
    handleSearch() {
      let todos = JSON.parse(localStorage.getItem(this.$STORAGE_KEY));
      if (todos && todos.length) {
        this.todos = todos.filter((todo) => {
          return todo.title.match(this.search);
        });
      }
    },
  },
};
</script>

<style>
@import "../src/assets/app.css";
</style>
