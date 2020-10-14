<template>
  <section class="main">
    <ul class="todo-list">
      <li
        v-for="todo in todos"
        v-bind:key="todo.id"
        v-bind:class="{
          completed: todo.completed,
          editing: todo == editedTodo,
        }"
      >
        <div class="view">
          <input class="toggle" type="checkbox" v-model="todo.completed" />
          <label v-on:dblclick="editTodo(todo)">{{ todo.title }}</label>
          <button class="destroy" v-on:click="removeTodo(todo)"></button>
        </div>
        <input
          class="edit"
          type="text"
          v-model="todo.title"
          v-on:blur="doneEdit(todo)"
          v-on:keyup.enter="doneEdit(todo)"
        />
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  name: "Todos",
  props: {
    STORAGE_KEY: {
      type: String,
      default: "",
    },
    todos: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
  data() {
    return {
      editedTodo: null,
    };
  },
  methods: {
    editTodo(todo) {
      this.editedTodo = todo;
    },
    doneEdit(todo) {
      if (!this.editedTodo) {
        return;
      }
      this.editedTodo = null;
      todo.title = todo.title.trim();
      if (!todo.title) {
        this.removeTodo(todo);
      }
      localStorage.setItem(this.STORAGE_KEY, JSON.stringify(this.todos));
    },
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
      localStorage.setItem(this.STORAGE_KEY, JSON.stringify(this.todos));
    },
  },
};
</script>

<style>
</style>