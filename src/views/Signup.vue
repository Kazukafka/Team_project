<template>
  <div class="SignupImage">
    <b-container>
      <div class="pt-5">
        <b-card class="m-auto" style="max-width: 25rem">
          <h1>{{ $t('header.signup') }} </h1>
          <b-form class="mx-auto" style="max-width: 30rem">
            <b-input-group class="mt-4">
              <b-form-input
              id = 'name-input'
                name="name"
                :placeholder="$t('info.name')"
                type="text"
                v-model="$v.form.name.$model"
                :state="validateState('name')"
              ></b-form-input>
            </b-input-group>

            <b-input-group class="mt-3">
              <b-form-input
              id = 'email-input'
                :placeholder="$t('info.email')"
                type="email"
                v-model="$v.form.email.$model"
                :state="validateState('email')"
              ></b-form-input>
            </b-input-group>

            <b-input-group class="mt-3">
              <b-form-input
                id="password-input"
                autocomplete="on"
                :placeholder="$t('info.password')"
                type="password"
                v-model="$v.form.password.$model"
                :state="validateState('password')"
              ></b-form-input>
              <b-form-invalid-feedback v-if="!$v.form.password.minLength"
                >Password must have at least
                {{
                  $v.form.password.$params.minLength.min
                }}
                characters.</b-form-invalid-feedback
              >
            </b-input-group>

            <b-input-group class="mt-3">
              <b-form-input
              id = 'homeAddress-input'
                :placeholder="$t('info.homeAddress')"
                type="text"
                v-model="$v.form.homeAddress.$model"
                :state="validateState('homeAddress')"
              ></b-form-input>
            </b-input-group>

            <b-input-group class="mt-3">
              <b-form-input
              id = 'phonenum-input'
                :placeholder="$t('info.telephoneNum')"
                type="number"
                v-model="$v.form.telephoneNumber.$model"
                :state="validateState('telephoneNumber')"
              ></b-form-input>
            </b-input-group>

            <b-button 
            id = 'btn-signup'
            variant="primary" 
            class="mt-3" 
             @click="submit($event)">{{ $t('header.signup') }} 
             </b-button>
          </b-form>
        </b-card>
      </div>
    </b-container>
  </div>
</template>

<script>
import { required, email, minLength, numeric } from "vuelidate/lib/validators";
export default {
  name: "Signup",

  data() {
    return {
      dismissSecs: 2,
      dismissCountDown: 0,
      showDismissibleAlert: false,
      form: {
        name: "",
        email: "",
        password: "",
        homeAddress: "",
        telephoneNumber: "",
      },
    };
  },
  validations: {
    form: {
      name: {
        required,
      },
      email: {
        required,
        email,
      },
      password: {
        required,
        minLength: minLength(8),
      },
      homeAddress: {
        required,
      },
      telephoneNumber: {
        numeric,
        required,
      },
    },
  },

  methods: {
    validateState(name) {
      const { $dirty, $error } = this.$v.form[name];
      return $dirty ? !$error : null;
    },
    submit(event) {
      this.$v.form.$touch();
      if (this.$v.$invalid) {
        event.preventDefault();
      } else {
        const formData = {
          email: this.form.email,
          password: this.form.password,
          name: this.form.name,
          homeAddress: this.form.homeAddress,
          telephoneNumber: this.form.telephoneNumber,
          status: this.form.status,
        };
        setTimeout(() => {
          this.dismissCountDown = this.dismissSecs;
        }, 700);
        this.$store.dispatch("signup", formData);
        setTimeout(() => {
          this.$router.push("/");
        }, 700);
        this.submitStatus = "OK";
      }
    },
  },
};
</script>

<style>
.SignupImage {
  background-image: url("https://image-cdn.essentiallysports.com/wp-content/uploads/20200316205830/Tennis-4.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  height: 100%;
  background-position: center;
}
</style>