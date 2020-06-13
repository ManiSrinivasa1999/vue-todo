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
              class="navbar-item"
            >
              <figure class="image is-48x48">
                <img
                  class="is-rounded  profile-picture"
                  src="https://bulma.io/images/placeholders/128x128.png"
                >
              </figure>
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
// import { dom } from '@fortawesome/vue-fontawesome';
import 'bulma';

export default {
  data() {
    return {

    };
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
