<template>
  <footer class="footer border-top" v-if="todos.length">
    <ul class="filters d-flex justify-content-center mt-3">
      <li>
        <a
          href="#"
          v-on:click="handleClick('all')"
          :class="{ selected: visibility == 'all' }"
          >All</a
        >
      </li>
      <li>
        <a
          href="#"
          v-on:click.prevent="handleClick('active')"
          :class="{ selected: visibility == 'active' }"
          >Active</a
        >
      </li>
      <li>
        <a
          href="#"
          v-on:click.prevent="handleClick('completed')"
          :class="{ selected: visibility == 'completed' }"
          >Completed</a
        >
      </li>
    </ul>
  </footer>
</template>

<script>
export default {
  name: "Footer",
  props: {
    todos: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      visibility: "all",
    };
  },
  methods: {
    handleClick(value) {
      this.$emit("handle-click", value);
      this.visibility = value;
    },
  },
};
</script>

<style>
.filters {
  list-style: none;
}
.filters li {
  margin-right: 15px;
}
.filters li a {
  color: gray;
  padding: 3px 7px;
  text-decoration: none;
  border: 1px solid transparent;
  border-radius: 3px;
}
.filters li a.selected,
.filters li a:hover {
  border: 1px solid gray;
}
</style>