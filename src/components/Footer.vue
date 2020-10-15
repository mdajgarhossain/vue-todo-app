<template>
  <footer class="footer">
    <ul class="filters">
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
</style>