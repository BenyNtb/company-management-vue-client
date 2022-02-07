<template>

  <v-app>
    <div>
      <v-app-bar color="deep-yellow accent-4" dense dark>
        <v-app-bar-nav-icon></v-app-bar-nav-icon>

        <v-toolbar-title>Page title</v-toolbar-title>

        <v-spacer></v-spacer>

        <div class="text-center">
          <v-btn @click="toggleOverlayRegister = true">
            Register
          </v-btn>
          <v-btn @click="toggleOverlayLogin = true">Sign in</v-btn>
        </div>
        <Register :toggleOverlayRegister="toggleOverlayRegister" @toggleOverlayRegister="toggleOverlayRegister = false"
          @registrationComplete="isConnected = true; getProfile()" />
        <Login :toggleOverlayLogin="toggleOverlayLogin" @toggleOverlayLogin="toggleOverlayLogin = false"
          @loginSuccess="getProfile()" />
      </v-app-bar>
    </div>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
  import Login from './components/Navbar/Login.vue';
  import Register from './components/Navbar/Register.vue'
  export default {
    components: {
      Login,
      Register
    },
    name: 'App',

    data() {
      return {
        toggleOverlayRegister: false,
        toggleOverlayLogin: false,
        isConnected: false,
        // displayedModalRegister: false,
      }
    }
  };
</script>