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
        class="border"
      >
        <div
          v-if="todo !== editedTodo"
          class="view d-flex justify-content-between align-items-center pl-3 pr-3"
        >
          <input
            @change="checkTodoStatus()"
            class="toggle"
            type="checkbox"
            v-model="todo.completed"
          />
          <label class="w-100 text-center" v-on:dblclick="editTodo(todo)">{{
            todo.title
          }}</label>
          <button
            type="button"
            class="destroy close"
            aria-label="Close"
            v-on:click="removeTodo(todo)"
          >
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <input
          v-if="todo == editedTodo"
          class="edit w-100"
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
    };
  },
  methods: {
    checkTodoStatus() {
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
.todo-list {
  margin: 0;
  padding: 0;
  list-style: none;
}
.todo-list li {
  font-size: 28px;
}
.todo-list li .toggle {
  width: 20px;
  height: 20px;
}
.todo-list li.completed label {
  color: #d9d9d9;
  text-decoration: line-through;
}
.todo-list li.editing {
  border-bottom: none;
  padding: 0;
}
.todo-list li.editing .view {
  display: none;
}
.todo-list li.editing .edit {
  display: block;
}
.todo-list li .edit {
  display: none;
}
</style>