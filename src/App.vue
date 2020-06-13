<template>
  <div class="container">
    <div class="here">
      <div class="hero-head">
        <div class="navbar">
          <div class="navbar-start">
            <h1 class="is-size-1">
              <strong>
                <router-link
                  to="/home"
                >
                  TODO
                </router-link>
              </strong>
            </h1>
          </div>
          <div class="navbar-menu">
            <div class="navbar-end">
              <div
                v-if="user"
                class="navbar-item has-dropdown is-hoverable"
              >
                <a class="navbar-link">
                  <figure class="image is-32x32">
                    <img class="is-rounded" :src="user.photoURL">
                  </figure>
                </a>

                <div class="navbar-dropdown is-right">
                  <a class="navbar-item"
                    @click="logout()"
                  >
                    Logout
                  </a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="hero-body">
        <router-view></router-view>
      </div>
      <!-- <div class="hero-body">
        <div class="modal"
          :class="!user ? 'is-active' : ''"
        >
          <div class="modal-background"></div>
          <div class="modal-content">
            <div class="card">
              <div class="card-header">
                <div class="container is-size-3 has-text-centered">
                  Please Login to Continue
                </div>
              </div>
              <div class="card-content">
                <div class="columns">
                  <div class="column">
                    <figure class="image is-48*48">
                      <img class="is-rounded" src="./assets/authentication.svg">
                    </figure>
                  </div>
                  <div class="column">
                    <div id="firebaseui-auth-container"></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div> -->
      <div class="hero-foot">
        <p class="has-text-centered">
          Done By Manisrinivasa &copy; 2020.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { dom } from '@fortawesome/fontawesome-svg-core';
import 'bulma';
import firebase from 'firebase/app';
import 'firebase/auth';
import 'firebase/firestore';
import 'firebaseui/dist/firebaseui.css';
import { firebaseConfig } from './config';

// eslint-disable-next-line @typescript-eslint/no-var-requires
const firebaseui = require('firebaseui');

firebase.initializeApp(firebaseConfig);
const ui = new firebaseui.auth.AuthUI(firebase.auth());

export default {
  mounted() {
    dom.watch();
    this.db.collection('todos')
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
        firebase.auth().onAuthStateChanged((user) => {
          if (user) {
            for (let i = 0; i < this.todos.length; i += 1) {
              if (this.todos[i].email === user.email) {
                const todo = user;
                this.user = user;
                this.$store.commit('setUser', todo);
                return;
              }
            }
            this.logout();
          } else {
            ui.start('#firebaseui-auth-container', {
              signInOptions: [
                firebase.auth.GoogleAuthProvider.PROVIDER_ID,
                firebase.auth.PhoneAuthProvider.PROVIDER_ID,
              ],
              // signInSuccessUrl: 'src/home',
              // Other config options...
            });
          }
        });
      });
  },
  methods: {
    logout() {
      firebase.auth().signOut();
      this.user = null;
    },
  },
  data() {
    return {
      user: null,
      users: [],
      db: firebase.firestore(),
    };
  },

};
</script>

<style>

</style>
