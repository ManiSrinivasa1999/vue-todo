<template>
  <div class="container">
    <div class="columns">
      <div class="column is-6">
        <div class="control">
          <input
            class="input is-focused"
            type="text"
            placeholder="Add tasks to todo"
            v-on:keyup.enter="addTodo"
          >
        </div>
      </div>
    </div>
    <div class="columns">
      <div class="column col-height">
        Todo
        <div
          v-for="(todo, index) in todos"
          :key="index"
          class="card"
        >
          <div class="card-content">
            {{ text }}
          </div>
          <footer class="card-footer">
            <a class="card-footer-item">Ongoing</a>
            <a class="card-footer-item">Complete</a>
            <a class="card-footer-item">Delete</a>
          </footer>
        </div>
      </div>
      <div class="column col-height">
        Ongoing
      </div>
      <div class="column col-height">
        Complete
      </div>
    </div>
  </div>
</template>

<script>
import 'bulma';

export default {
  data() {
    return {
      todos: [
        {
          text: '',
        },
      ],
    };
  },
  methods: {
    addTodo(event) {
      const text = event.target.value;
      this.todos.push({ text });
      // eslint-disable-next-line no-param-reassign
      event.target.value = '';
    },
  },
};
</script>

<style>
.col-height {
  height: 75vmin;
}
</style>
