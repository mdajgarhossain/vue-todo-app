<template>
  <div id="app">
    <section class="todoapp">
      <Search @searchMatchedData="embedMatchedData" />
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
import Search from "./components/Search";

export default {
  name: "App",
  components: {
    Header,
    Todos,
    Footer,
    Search,
  },
  data() {
    return {
      todos: [],
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

  methods: {
    saveTodos(todos) {
      this.todos = [...todos];
    },
    checkVisibility(value) {
      this.visibility = value;
    },
    embedMatchedData(matchedData) {
      this.todos = matchedData;
    },
  },
};
</script>

<style>
@import "../src/assets/app.css";
</style>
