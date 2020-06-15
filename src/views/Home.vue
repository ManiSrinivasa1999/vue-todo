<template>
  <div class="container">
    <section class="hero">
      <div class="hero-body">
        <div class="columns">
          <div class="column is-6">
            <div class="field has-addons">
              <div class="control is-expanded has-icons-left">
                <input
                  class="input is-rounded is-focused"
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
            <h2 class="is-size-2 has-text-centered has-text-danger">
              Todo
            </h2>
            <div class="todo-list">
              <div
                v-for="(todoItem, index) in todosList"
                :key="index"
                class="card"
              >
                <div class="card-content">
                  <p class="card-header-title">
                  {{ todoItem.text }}
                  </p>
                </div>
                  <footer class="card-footer">
                  <button
                    class="card-footer-item button is-primary"
                    @click="onGoingList(todoItem.id)"
                  >
                    Ongoing
                  </button>
                  <button
                    class="card-footer-item button is-success"
                    @click="completeList(todoItem.id)"
                  >
                    Complete
                  </button>
                  <button
                    class="card-footer-item button is-danger"
                    @click="deleteTodo(todoItem.id)"
                  >
                    Delete
                  </button>
                </footer>
              </div>
            </div>
          </div>
          <div class="column col-height">
            <h2 class="is-size-2 has-text-centered has-text-link">
              Ongoing
            </h2>
            <div class="ongoing-list">
              <div
                v-for="(ongoingItem, index) in ongoingLists"
                :key="index"
                class="card"
              >
                <div class="card-content">
                  <p class="card-header-title">
                  {{ ongoingItem.text }}
                  </p>
                </div>
                  <footer class="card-footer">
                  <button
                    class="card-footer-item button is-success"
                    @click="completeList(ongoingItem.id)"
                  >
                    Complete
                  </button>
                  <button
                    class="card-footer-item button is-danger"
                    @click="deleteTodo(ongoingItem.id)"
                  >
                    Delete
                  </button>
                </footer>
              </div>
            </div>
          </div>
          <div class="column col-height">
            <h2 class="is-size-2 has-text-centered has-text-success">
              Complete
            </h2>
            <div class="complete-list">
              <div
                v-for="(completeItem, index) in completeLists"
                :key="index"
                class="card"
              >
                <div class="card-content">
                  <p class="card-header-title">
                  {{ completeItem.text }}
                  </p>
                </div>
                <footer class="card-footer">
                  <button
                    class="card-footer-item button is-danger"
                    @click="deleteTodo(completeItem.id)"
                  >
                    Delete
                  </button>
                </footer>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { dom } from '@fortawesome/fontawesome-svg-core';
import 'bulma';
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';
import { firebaseConfig } from '../config';

// eslint-disable-next-line @typescript-eslint/no-var-requires

firebase.initializeApp(firebaseConfig);

export default {
  data() {
    return {
      todos: [],
      tasks: {
        todo: [],
        ongoing: [],
        completed: [],
      },
      db: firebase.firestore(),
    };
  },
  computed: {
    todosList() {
      const todos = [];
      this.todos.forEach((todo) => {
        if (todo.status === 'todo') {
          todos.push(todo);
        }
      });
      return todos;
    },
    ongoingLists() {
      const ongoing = [];
      this.todos.forEach((todo) => {
        if (todo.status === 'ongoing') {
          ongoing.push(todo);
        }
      });
      return ongoing;
    },
    completeLists() {
      const complete = [];
      this.todos.forEach((todo) => {
        if (todo.status === 'complete') {
          complete.push(todo);
        }
      });
      return complete;
    },
  },
  mounted() {
    dom.watch();
    this.db.collection('storeTasks')
      .onSnapshot((docs) => {
        const todos = [];
        docs.forEach((doc) => {
          if (doc.exists) {
            const todo = doc.data();
            todo.id = doc.id;
            todos.push(todo);
          }
        });
        this.todos = todos;
      });
  },
  methods: {
    addTodo(event) {
      const text = event.target.value;
      const status = 'todo';
      this.todos.push({ text, status });
      // eslint-disable-next-line no-param-reassign
      event.target.value = '';
      this.db.collection('storeTasks')
        .add({
          text,
          status,
        })
        .then()
        .catch();
    },
    deleteTodo(id) {
      this.db.collection('storeTasks')
        .doc(id)
        .delete();
    },
    onGoingList(id) {
      this.db.collection('storeTasks')
        .doc(id)
        .update({
          status: 'ongoing',
        });
    },
    completeList(id) {
      this.db.collection('storeTasks')
        .doc(id)
        .update({
          status: 'complete',
        });
    },
  },
};
</script>

<style>
.col-height {
  height: 75vmin;
}
</style>
