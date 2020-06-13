<template>
  <div class="container">
    <section class="hero">
      <div class="hero-body">
        <div class="columns">
          <div class="column is-6">
            <div class="field has-addons">
              <div class="control is-expanded has-icons-left">
                <input
                  class="input is-rounded"
                  type="text"
                  placeholder="Add your task to todo"
                  v-on:keyup.enter="addTaskToTodo"
                >
                <span class="icon is-small is-left">
                  <i class="fas fa-list"></i>
                </span>
              </div>
            </div>
          </div>
        </div>
        <div class="columns">
          <div class="column col-height">
            Todo
            <ul>
              <li
                v-for="(todo , index) in todos"
                :key="index"
              >
                <div class="card">
                  <div class="card-header">
                    {{ text.value }}
                  </div>
                  <div class="card-footer"></div>
                </div>
              </li>
            </ul>
          </div>
          <div class="column col-height">
            Ongoing
          </div>
          <div class="column col-height">
            Complete
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// import { dom } from '@fortawesome/vue-fontawesome';
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
    addTaskToTodo(event) {
      const text = event.target.value;
      this.todos.push({ text });
      text.value = '';
    },
  },
};
</script>>

<style lang="scss" scoped>
.profile-picture {
  max-height: 48px;
}

.col-height {
  height: 50vmin;
}

h1 {
font-family: 'Sriracha', cursive;
}

@import url('https://fonts.googleapis.com/css2?family=Metal+Mania&display=swap');
</style>
