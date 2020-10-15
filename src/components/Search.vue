<template>
  <div class="search">
    <input type="text" v-model="search" placeholder="Search here" />
    <span>{{ search }}</span>
  </div>
</template>

<script>
export default {
  name: "Search",
  data() {
    return {
      search: "",
    };
  },
  methods: {
    handleSearch() {
      let todos = JSON.parse(localStorage.getItem(this.$STORAGE_KEY));
      if (todos && todos.length) {
        let matchedTodos = todos.filter((todo) => {
          return todo.title.match(this.search);
        });
        this.$emit("searchMatchedData", matchedTodos);
      }
    },
  },
  watch: {
    search: {
      deep: true,
      handler(n, o) {
        if (o != n) {
          this.handleSearch();
        }
      },
    },
  },
};
</script>

<style>
</style>