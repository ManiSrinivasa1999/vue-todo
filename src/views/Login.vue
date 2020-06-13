<template>
  <div class="container">
    <div class="columns">
      <div class="column">
        <figure class="image">
          <img
            src="../assets/authentication.svg"
            alt="Authentication"
          >
        </figure>
      </div>
      <div class="column">
        <h2 class="is-size-1 has-text-centered">
          Login to your panel
        </h2>
        <div id="firebaseui-auth-container"></div>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';
import 'firebaseui/dist/firebaseui.css';
import { firebaseConfig } from '../config';

// eslint-disable-next-line @typescript-eslint/no-var-requires
const firebaseui = require('firebaseui');

firebase.initializeApp(firebaseConfig);

// Initialize the FirebaseUI Widget using Firebase.
const ui = new firebaseui.auth.AuthUI(firebase.auth());

export default {
  data() {
    return {
      user: null,
      db: firebase.firestore,
    };
  },
  mounted() {
    this.db.collection('users')
      .onSnapshot((docs) => {
        const users = [];
        docs.forEach((doc) => {
          if (doc.exists) {
            const user = doc.data();
            user.id = doc.id;
            users.push(user);
          }
        });
        this.users = users;
        firebase.auth().onAuthStateChanged((user) => {
          if (user) {
            for (let i = 0; i < this.users.length; i += 1) {
              if (this.users[i].email === user.email) {
                this.user = user;
                this.$store.commit('setUser', user);
                return;
              }
            }
          } else {
            ui.start('#firebaseui-auth-container', {
              signInOptions: [
                firebase.auth.GoogleAuthProvider.PROVIDER_ID,
                firebase.auth.PhoneAuthProvider.PROVIDER_ID,
              ],
              // Other config options...
            });
          }
        });
      });
  },
};
</script>
