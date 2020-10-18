<template>
  <footer class="footer border-top p-2 justify-self-center">
    <ul class="filters d-flex justify-content-center justify-content-around">
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
    visibility: {
      type: String,
    },
  },
  methods: {
    handleClick(value) {
      this.$emit("handleClick", value);
    },
  },
};
</script>

<style>
.filters {
  margin: 0;
  padding: 0;
  list-style: none;
}
</style>