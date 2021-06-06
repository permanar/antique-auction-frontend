<template>
  <div>
    <form>
      <v-text-field
        v-model="email"
        :error-messages="emailErrors"
        label="E-mail"
        required
        @input="$v.email.$touch()"
        @blur="$v.email.$touch()"
      ></v-text-field>

      <v-text-field
        v-model="password"
        :error-messages="passwordErrors"
        label="Password"
        type="password"
        required
        @input="$v.password.$touch()"
        @blur="$v.password.$touch()"
      ></v-text-field>
      <v-btn class="mr-4" @click="login" :loading="loading">
        Login
      </v-btn>

      <v-snackbar v-model="snackbar" :timeout="timeout">
        {{ snackbarText }}

        <template v-slot:action="{ attrs }">
          <v-btn color="blue" text v-bind="attrs" @click="snackbar = false">
            Close
          </v-btn>
        </template>
      </v-snackbar>
    </form>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, email, minLength } from "vuelidate/lib/validators";

export default {
  layout: "admin",
  mixins: [validationMixin],
  validations: {
    email: { required, email },
    password: { required, minLength: minLength(6) }
  },
  data() {
    return {
      title: "Login",
      email: "",
      password: "",
      loading: false,
      submitMessage: null,
      snackbar: false,
      snackbarText: "",
      timeout: 4000
    };
  },
  computed: {
    emailErrors() {
      const errors = [];
      if (!this.$v.email.$dirty) return errors;
      !this.$v.email.email && errors.push("Must be valid e-mail");
      !this.$v.email.required && errors.push("E-mail is required");
      return errors;
    },
    passwordErrors() {
      const errors = [];
      if (!this.$v.password.$dirty) return errors;
      !this.$v.password.required && errors.push("Password is required");
      return errors;
    }
  },
  methods: {
    async login() {
      this.loading = true;
      this.$v.$touch();
      if (this.$v.$invalid) {
        this.submitMessage = "Please complete the form first!";
        return;
      }
      await this.$auth
        .login({
          data: {
            email: this.email,
            password: this.password
          }
        })
        .then(() => {
          this.$router.push("/admin");
        })
        .catch(err => {
          this.snackbarText = "Login failed";
          this.snackbar = true;
          console.error("Error: ", err);
        })
        .finally(() => {
          setTimeout(() => {
            this.loading = false;
          }, 1000);
        });
    }
  }
};
</script>
