<template>
  <div id="app">
    <section class="container d-flex justify-content-center mt-3 pt-3">
      <div class="all-components shadow p-3 mb-5 bg-white rounded">
        <Search @searchMatchedData="embedMatchedData" />
        <Header @save-todos="saveTodos" :todos="todos" />
        <Todos :todos="todos" :filteredTodos="filteredTodos" />
        <Footer @handleClick="checkVisibility" :todos="todos" />
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
/* @import "../src/assets/app.css"; */
html,
#app {
  margin: 0;
  padding: 0;
  /* background: #d0e6ff; */
}
.all-components {
  /* background: #46529c; */
}
</style>
