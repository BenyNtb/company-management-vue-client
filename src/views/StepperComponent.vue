<template>
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

    </v-stepper-header>

    <v-stepper-items>
      <v-stepper-content step="1">
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field v-model="tva" label="TVA" required></v-text-field>
          <v-text-field :value="company" label="Company Name" required disabled></v-text-field>
          <v-text-field :value="activity" label="Company Activity" required disabled></v-text-field>
          <v-text-field :value="address" label="Address" required disabled></v-text-field>
          <v-text-field :value="city" label="City" required disabled></v-text-field>
          <v-text-field :value="phone" label="Phone Number" required disabled></v-text-field>
          <v-text-field :value="postal" label="Postal Code" required disabled></v-text-field>
          <v-text-field :value="country" label="Country" required disabled></v-text-field>



          <!-- <v-select
      v-model="select"
      :items="items"
      :rules="[v => !!v || 'Item is required']"
      label="Item"
      required
    ></v-select> -->


          <v-btn :disabled="!valid" color="success" class="mr-4" @click="getTva">
            Validate
          </v-btn>

          <v-btn color="error" class="mr-4" @click="reset">
            Reset Form
          </v-btn>

          <v-btn color="warning" @click="resetValidation">
            Reset Validation
          </v-btn>
        </v-form>

        <v-btn color="primary" @click="e1 = 2">
          Continue
        </v-btn>

        <v-btn text>
          Cancel
        </v-btn>
      </v-stepper-content>

      <v-stepper-content step="2">
        <v-text-field v-model="email" :rules="emailRules" label="Contact Person Mail" required></v-text-field>
        <v-text-field v-model="name" :rules="nomRules" label="Contact Person Name" required></v-text-field>
        <v-text-field v-model="number" :rules="numeroRules" label="Contact Person Number" required></v-text-field>

        <v-btn color="primary" @click="completeProfile">
          Continue
        </v-btn>

        <v-btn text>
          Cancel
        </v-btn>
      </v-stepper-content>
    </v-stepper-items>
  </v-stepper>
</template>

<script>
  import axios from 'axios'
  import {
    mapState
  } from "vuex"
  export default {
    data() {
      return {
        e1: 1,
        valid: true,
        tva: '',
        company: '',
        activity: '',
        address: '',
        city: '',
        phone: '',
        postal: '',
        email: '',
        name: '',
        number: '',
        country: '',
        emailRules: [
          (v) => !!v || "E-mail is required",
          (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
        ],
        nomRules: [
          (v) => !!v || "Le numéro est requis",
          (v) =>
          (v && v.length >= 2) || "Le nom doit contenir au moins 8 caractères",
        ],
        numeroRules: [
          (v) => !!v || "Le numéro est requis",
          (v) =>
          (v && v.length >= 9) || "Le numéro doit contenir au moins 9 chiffres",
        ],
      }
    },
    methods: {
      getTva() {
        axios.get(`http://13.38.138.92/api/companies/${this.tva}/info `).then(res => {
          console.log(res.data.data)
          this.company = res.data.data.nom
          this.activity = res.data.data.activite
          this.address = res.data.data.adresse
          this.city = res.data.data.ville
          this.phone = res.data.data.numero
          this.postal = res.data.data.code_postal
          this.country = res.data.data.pays
        })
      },
      completeProfile() {
        let formdata = new FormData();
        formdata.append('tva', this.tva)
        formdata.append('nom', this.company)
        formdata.append('activite', this.activity)
        formdata.append('adresse', this.address)
        formdata.append('ville', this.city)
        formdata.append('code_postal', this.postal)
        formdata.append('numero', this.phone)
        formdata.append('email_contact', this.email)
        formdata.append('nom_contact', this.name)
        formdata.append('numero_contact', this.number)
        formdata.append('pays', this.country)
        console.log(this.token)
        axios.post(`http://127.0.0.1:8000/api/profilstore`, formdata, {
          headers: {
            Authorization: "Bearer " + this.token
          }
        }).then(res => {
          console.log(res)
          this.$router.push("/")
        })
      },
      validate() {
        this.$refs.form.validate()
      },
      reset() {
        this.$refs.form.reset()
      },
      resetValidation() {
        this.$refs.form.resetValidation()
      },
    },
    computed: {
      ...mapState(['user', 'token'])
    },
  }
</script>