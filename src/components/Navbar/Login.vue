<template>
  <v-row justify="center">
    <v-dialog v-model="toggleOverlayLogin" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="text-h5">User Profile</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  label="Email"
                  v-model="email"
                  type="email"
                  required
                  :rules="emailRules"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="password"
                  :append-icon="isPasswordVisible == true ? 'mdi-eye-off' : 'mdi-eye'"
                  :type="isPasswordVisible == true ? 'text' : 'password'"
                  @click:append="isPasswordVisible = !isPasswordVisible"
                  :rules="passwordRules"
                  label="Password"
                  hint="Greater than 10 characters and one character in upper case"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="$emit('toggleOverlayLogin', false)">Close</v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="login() ; $emit('toggleOverlayLogin', false)"
          >Login</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
import axios from 'axios'
export default {
  props: {
    toggleOverlayLogin: { type: Boolean }
  },
  name: 'LoginOverlay',
  data: () => ({
    isPasswordVisible: false,
    email: null,
    password: null,
    passwordRules: [
      v => (!!v && v.length > 5) || 'The password must be comported minimum 5 characters',
      v => (v && v != v.toLowerCase()) || 'The password must be comported minimum one character in upper case',
    ],
    emailRules: [
      v => (!!v && v.length > 5) || 'The email must be comported minimum 5 characters',
      v => (v && v.indexOf('@') > 0) || 'The email must be comported @ character',
    ],
  }),
  methods: {
    login () {
      let formData = new FormData;
      formData.append('email', this.email)
      formData.append('password', this.password)
      formData.append('password_confirmation', this.password)
      formData.append('confirmation_password', this.password)

      axios.get('/sanctum/csrf-cookie').then(() => {
        axios.post('/api/v1/login', formData).then(res => {
          localStorage.setItem('bearerToken', res.data.data.bearerToken);
          localStorage.setItem('userId', res.data.data.user.id)
          location.reload();
        })
      })
    },
  }
}
</script>

<style>
</style>