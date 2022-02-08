<template>
    <v-card-tex>
        <v-container fluid>
              <v-col
          cols="12"
          sm="6"
        >
            <v-text-field v-model="connexion.email" :rules="emailRules" label="E-mail" name="email" required>
            </v-text-field>

            <v-text-field v-model="connexion.password" label="Password" name="password" required
                @keypress.enter="login()"></v-text-field>

            <v-btn color="success" @click="connected()" >
                Login
            </v-btn>
        </v-col>
        </v-container>

    </v-card-tex>
</template>

<script>
    export default {
        name: "Login",
        data: () => ({

            valid: true,
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+\..+/.test(v) || 'E-mail must be valid',

            ],
            connexion: {
                email: null,
                password: null
            }

        }),

        methods: {
            connected() {
                this.$store.dispatch('login', this.connexion)
                this.$router.push({
                    name: 'Home'
                })
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>