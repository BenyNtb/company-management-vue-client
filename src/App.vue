<template>
  
  <v-app>
    <div>
    <v-app-bar
      color="deep-yellow accent-4"
      dense
      dark
    >
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>Page title</v-toolbar-title>

      <v-spacer></v-spacer>

      <div class="text-center">
    <v-btn
      class="ma-2"
      :loading="loading"
      :disabled="loading"
      color="secondary"
      @click="loader = 'loading'"
    >
      Sign Up
    </v-btn>
    <v-btn
      class="ma-2"
      :loading="loading"
      :disabled="loading"
      color="secondary"
      @click="displayedModalLogin = !displayedModalLogin"
    >
      Sign In
    </v-btn>
  </div>

      <v-menu
        left
        bottom
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            icon
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>

        <v-list>
          <v-list-item
            v-for="n in 5"
            :key="n"
            @click="() => {}"
          >
            <v-list-item-title>Option {{ n }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
      <Login v-show="displayedModalLogin" @toggleLoginOverlay="displayedModalLogin = !displayedModalLogin"/>
    </v-app-bar>
  </div>
    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
import Login from './components/Navbar/Login.vue';
export default {
  components: {
    Login,
  },
  name: 'App',

  data () {
    return {
      displayedModalLogin: false,
      displayedModalRegister: false,
    }
  }
};
</script>
