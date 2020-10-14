<template>
  <div id="app">
    <section class="todoapp">
      <!-- <input type="text" v-model="search" placeholder="Search here" />
      <span>{{ search }}</span> -->

      <Header @save-todos="saveTodos" :todos="todos" />
      <Todos :todos="todos" />
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
import Todos from "./components/Todos";

// const STORAGE_KEY = "todo-storage";
export default {
  name: "App",
  components: {
    Header,
    Todos,
  },
  data() {
    return {
      todos: [],
      visibility: "all",
      search: "",
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
    saveTodos(todos) {
      this.todos = [...todos];
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
