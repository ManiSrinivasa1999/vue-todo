<template>
  <div class="container">
    <div class="here">
      <div class="hero-head">
        <div class="navbar">
          <div class="navbar-start">
            <div class="navbar-brand">
              <h1 class="is-size-1">
                <strong>TODO</strong>
              </h1>
            </div>
          </div>
          <div class="navbar-end">
            <div
              v-if="$store.state.user"
              class="navbar-item has-dropdown is-hoverable"
            >
              <figure class="image is-48x48">
                <img
                  class="is-rounded  profile-picture"
                  :src="$store.state.user.photoURL"
                >
              </figure>
              <div class="navbar-dropdown is-right">
                <p class="navbar-item">
                  {{ $store.state.user.displayName }}
                </p>
                <p class="navbar-item">
                  {{ $store.state.user.email }}
                </p>
                <a class="navbar-item"
                  @click="logout()"
                >
                  Logout
                </a>
              </div>
            </div>
            <div
              v-else
              class="navbar-item"
            >
              <router-link
                to="/login"
              >
                Login
              </router-link>
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
import 'bulma';
import { dom } from '@fortawesome/fontawesome-svg-core';
import firebase from 'firebase/app';

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
            this.$router.push('/login');
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
</script>>

<style lang="scss" scoped>

.profile-picture {
  max-height: 48px;
}

</style>
