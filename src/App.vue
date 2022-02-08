<template>

  <v-app>
    <div>
      <v-app-bar color="deep-yellow accent-4" dense dark>
        <v-app-bar-nav-icon></v-app-bar-nav-icon>

        <v-toolbar-title>Page title</v-toolbar-title>

        <v-spacer></v-spacer>

        <div class="text-center">
          <v-btn>{{user.name}}</v-btn>
          <v-btn v-if="token != null" @click="logout" class="mr-3">Logout</v-btn>
          <v-btn v-if="token == null" href="/login">Sign in</v-btn>
        </div>
      </v-app-bar>
    </div>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
  import {
    mapState
  } from "vuex"
  export default {
    computed: {
      ...mapState(['user', 'token'])
    },

    name: 'App',
    methods: {
      logout() {
        this.$store.dispatch('logout');
        this.$router.push({
          name: "Home"
        });

      }
    },

  };
</script>