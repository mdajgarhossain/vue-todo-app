<template>
  <div id="app">
    <section class="container d-flex justify-content-sm-center mt-3 pt-3">
      <div class="all-components shadow p-3 mb-5 bg-white rounded">
        <h1
          class="app-title display-3 font-weight-bold font-italic text-muted text-center"
        >
          todos
        </h1>
        <Search :hasTodo="hasTodo" @search-matched-data="embedMatchedData" />
        <Header @save-todos="saveTodos" :todos="todos" />
        <Todos :todos="todos" :filteredTodos="filteredTodos" />
        <Footer @handle-click="checkVisibility" :todos="todos" />
      </div>
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
      date: "",
      time: "",
      hasTodo: false,
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
    todos: {
      immediate: true,
      deep: true,
      handler() {
        if (this.todos && this.todos.length) {
          this.hasTodo = true;
        } else {
          this.hasTodo = false;
        }
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
    embedMatchedData(matchedData) {
      this.todos = matchedData;
    },
  },
};
</script>

<style>
html,
#app {
  margin: 0;
  padding: 0;
  background: #f8f9fa;
}
</style>
