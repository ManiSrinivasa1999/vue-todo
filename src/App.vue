<template>
  <div class="container">
    <section class="hero">
      <div class="hero-head">
        <nav class="navbar">
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
        </nav>
      </div>
      <div class="hero-body">
        <router-view></router-view>
      </div>
      <div class="hero-foot has-text-centered">
        Done By Manisrinivasa &copy;2020
      </div>
    </section>
  </div>
</template>

<script>
import 'bulma';
import firebase from 'firebase/app';

export default {
  data() {
    return {

    };
  },
  methods: {
    logout() {
      firebase.auth().signOut();
      this.user = null;
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
