<template>
  <div class="search d-flex justify-content-end pb-2">
    <div>
      <input
        type="text"
        v-model="search"
        placeholder="Search to do"
        class="border border-secondary pl-2"
      />
      <p
        class="text-danger alert alert-warning"
        role="alert"
        v-if="!hasTodo && search"
      >
        No search results!
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Search",
  props: {
    hasTodo: {
      type: Boolean,
      default: true,
    },
  },
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
        this.$emit("search-matched-data", matchedTodos);
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

<style scoped>
.search input::-webkit-input-placeholder {
  color: #a8a6a6;
  font-style: italic;
}
.search input {
  font-size: 18px;
  border-radius: 15px;
}
.search input:focus {
  outline-style: none;
}
</style>