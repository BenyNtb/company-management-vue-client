<template>
    <v-row justify="center">
        <v-dialog v-model="toggleOverlayRegister" persistent max-width="600px">
            <v-stepper v-model="e1">
                <v-stepper-header>
                    <v-stepper-step :complete="e1 > 1" step="1">
                        Name of step 1
                    </v-stepper-step>
                    <v-divider></v-divider>
                    <v-stepper-step :complete="e1 > 2" step="2">
                        Name of step 2
                    </v-stepper-step>
                    <v-divider></v-divider>
                    <v-stepper-step :complete="e1 > 3" step="3">
                        Name of step 3
                    </v-stepper-step>
                </v-stepper-header>
                <v-stepper-items>
                    <v-stepper-content step="1">
                        <v-card>
                            <v-card-title>
                                <span class="text-h5">Mail</span>
                            </v-card-title>
                            <v-card-text>
                                <v-container>
                                    <v-row>
                                        <v-col cols="12">
                                            <v-text-field label="Email" v-model="email" type="email" required
                                                :rules="emailRules"></v-text-field>
                                        </v-col>
                                        <v-col cols="12">
                                            <v-text-field v-model="password"
                                                :append-icon="isPasswordVisible == true ? 'mdi-eye-off' : 'mdi-eye'"
                                                :type="isPasswordVisible == true ? 'text' : 'password'"
                                                @click:append="isPasswordVisible = !isPasswordVisible"
                                                :rules="passwordRules" label="Password"
                                                hint="Greater than 10 characters and one character in upper case"
                                                required></v-text-field>
                                        </v-col>
                                        <v-btn @click="validate" color="success">Create account</v-btn>
                                    </v-row>
                                </v-container>
                            </v-card-text>
                            <v-btn color="primary" @click="e1 = 2">
                                Continue
                            </v-btn>
                            <v-btn text @click="$emit('toggleOverlayRegister', false)">
                                Cancel
                            </v-btn>
                        </v-card>
                    </v-stepper-content>
                    <v-stepper-content step="2">
                        <v-form ref="form" v-model="valid" lazy-validation>
                            <v-text-field v-model="name" :counter="10" :rules="nameRules" label="Company Name" required>
                            </v-text-field>
                            <v-text-field v-model="name" :counter="10" :rules="nameRules" label="Company Activity"
                                required>
                            </v-text-field>
                            <v-text-field v-model="name" :counter="10" :rules="nameRules" label="Address" required>
                            </v-text-field>
                            <v-text-field v-model="name" :counter="10" :rules="nameRules" label="City" required>
                            </v-text-field>
                            <v-text-field v-model="name" :counter="10" :rules="nameRules" label="Phone Number" required>
                            </v-text-field>
                            <v-text-field v-model="name" :counter="10" :rules="nameRules" label="Postal Code" required>
                            </v-text-field>
                            <!-- <v-select
      v-model="select"
      :items="items"
      :rules="[v => !!v || 'Item is required']"
      label="Item"
      required
    ></v-select> -->
                            <!-- <v-checkbox v-model="checkbox" :rules="[v => !!v || 'You must agree to continue!']"
                                label="Do you agree?" required></v-checkbox> -->

                            <v-btn :disabled="!valid" color="success" class="mr-4" @click="validate">
                                Validate
                            </v-btn>
                        </v-form>
                        <v-btn color="primary" @click="e1 = 3">
                            Continue
                        </v-btn>
                        <v-btn text @click="$emit('toggleOverlayRegister', false)">
                            Cancel
                        </v-btn>
                    </v-stepper-content>
                    <v-stepper-content step="3">
                        <v-text-field v-model="email" :rules="emailRules" label="Contact Person Mail" required>
                        </v-text-field>
                        <v-text-field v-model="email" :rules="emailRules" label="Contact Person Name" required>
                        </v-text-field>
                        <v-text-field v-model="email" :rules="emailRules" label="Contact Person Number" required>
                        </v-text-field>
                        <v-btn color="primary" @click="e1 = 3">
                            Continue
                        </v-btn>
                        <v-btn text>
                            Cancel
                        </v-btn>
                    </v-stepper-content>
                </v-stepper-items>
            </v-stepper>
        </v-dialog>
    </v-row>
</template>

<script>
    import axios from 'axios'
    // import Vue from 'vue'
    export default {
        props: {
            toggleOverlayRegister: {
                type: Boolean
            }
        },
        name: 'Register',
        data() {
            return {
                e1: 1,
                valid: true,
                name: '',
                nameRules: [
                    v => !!v || 'Name is required',
                    v => (v && v.length <= 10) || 'Name must be less than 10 characters',
                ],
                email: '',
                emailRules: [
                    v => !!v || 'E-mail is required',
                    v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
                ],
                select: null,
                items: [
                    'Item 1',
                    'Item 2',
                    'Item 3',
                    'Item 4',
                ],
                checkbox: false,
            }
        },

        methods: {
            //INSCRIPTION USING API
            register() {
                this.succes = [];
                this.errors = [];
                let formData = new FormData();
                formData.append("name", this.name);
                formData.append("email", this.email);
                formData.append("password", this.password);
                let rout = this.$router;
                axios.post('http://127.0.0.1:8000/api/register', formData, {
                        headers: {
                            "Access-Control-Allow-Origin": "*",
                            "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                            "Access-Control-Allow-Headers": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                        }
                    })
                    .then(res => {
                        if (res.data.status == 200) {
                            this.$store.dispatch('userConnected', res.data)
                            rout.push('/dashboard');
                        }
                    })
                    .catch(err => {
                        console.log(err)
                        this.$toast.error(`${err.response.data.message}`, {
                            position: 'top-right',
                            duration: 2000,
                            dismissible: true
                        });
                    });
            },
            //METHOD TO CHECK IF THE API MATCH WITH THE DB
            tvaCheck() {
                axios.get(`http://127.0.0.1:8000/api/companies/${this.company.tva}/info`).then(res => {
                    this.company = res.data.data
                    console.log(res.data.data)
                })
            }
        }
    };
</script>

<style>

</style>