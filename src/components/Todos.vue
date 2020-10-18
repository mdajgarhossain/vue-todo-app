<template>
  <section class="main">
    <ul class="todo-list">
      <li
        v-for="todo in filteredTodos"
        v-bind:key="todo.id"
        v-bind:class="{
          completed: todo.completed,
          editing: todo == editedTodo,
        }"
      >
        <div class="view">
          <input
            @change="checkTodoStatus()"
            class="toggle"
            type="checkbox"
            v-model="todo.completed"
          />
          <label v-on:dblclick="editTodo(todo)">{{ todo.title }}</label>
          <button class="destroy" v-on:click="removeTodo(todo)"></button>
        </div>
        <input
          class="edit"
          type="text"
          v-model="todo.title"
          v-on:blur="completeEdit(todo)"
          v-on:keyup.enter="completeEdit(todo)"
        />
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  name: "Todos",
  props: {
    todos: {
      type: Array,
      default: () => {
        return [];
      },
    },
    filteredTodos: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
  data() {
    return {
      editedTodo: null,
      // toggle: false,
    };
  },
  methods: {
    checkTodoStatus() {
      // console.log(todo);
      // this.toggle = true;
      localStorage.setItem(this.$STORAGE_KEY, JSON.stringify(this.todos));
    },
    editTodo(todo) {
      this.editedTodo = todo;
    },
    completeEdit(todo) {
      if (!this.editedTodo) {
        return;
      }
      this.editedTodo = null;
      todo.title = todo.title.trim();
      if (!todo.title) {
        this.removeTodo(todo);
      }
      localStorage.setItem(this.$STORAGE_KEY, JSON.stringify(this.todos));
    },
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
      localStorage.setItem(this.$STORAGE_KEY, JSON.stringify(this.todos));
    },
  },
};
</script>

<style>
</style>