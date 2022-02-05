<template>
  <v-stepper v-model="e1">
    <v-stepper-header>
      <v-stepper-step
        :complete="e1 > 1"
        step="1"
      >
        Name of step 1
      </v-stepper-step>

      <v-divider></v-divider>

      <v-stepper-step
        :complete="e1 > 2"
        step="2"
      >
        Name of step 2
      </v-stepper-step>

      <v-divider></v-divider>

    </v-stepper-header>

    <v-stepper-items>
      <v-stepper-content step="1">
        <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Company Name"
      required
    ></v-text-field>
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Company Activity"
      required
    ></v-text-field>
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Address"
      required
    ></v-text-field>
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="City"
      required
    ></v-text-field>
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Phone Number"
      required
    ></v-text-field>
    <v-text-field
      v-model="name"
      :counter="10"
      :rules="nameRules"
      label="Postal Code"
      required
    ></v-text-field>

    

    <!-- <v-select
      v-model="select"
      :items="items"
      :rules="[v => !!v || 'Item is required']"
      label="Item"
      required
    ></v-select> -->

    <v-checkbox
      v-model="checkbox"
      :rules="[v => !!v || 'You must agree to continue!']"
      label="Do you agree?"
      required
    ></v-checkbox>

    <v-btn
      :disabled="!valid"
      color="success"
      class="mr-4"
      @click="validate"
    >
      Validate
    </v-btn>

    <v-btn
      color="error"
      class="mr-4"
      @click="reset"
    >
      Reset Form
    </v-btn>

    <v-btn
      color="warning"
      @click="resetValidation"
    >
      Reset Validation
    </v-btn>
  </v-form>

        <v-btn
          color="primary"
          @click="e1 = 2"
        >
          Continue
        </v-btn>

        <v-btn text>
          Cancel
        </v-btn>
      </v-stepper-content>

      <v-stepper-content step="2">
        <v-text-field
      v-model="email"
      :rules="emailRules"
      label="Contact Person Mail"
      required
    ></v-text-field>
    <v-text-field
      v-model="email"
      :rules="emailRules"
      label="Contact Person Name"
      required
    ></v-text-field>
    <v-text-field
      v-model="email"
      :rules="emailRules"
      label="Contact Person Number"
      required
    ></v-text-field>

        <v-btn
          color="primary"
          @click="e1 = 3"
        >
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
 export default {
    data () {
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
      validate () {
        this.$refs.form.validate()
      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },
    },
  }
</script>