<template>
  <div class="row mt-5">
    <div class="col-md-2"></div>
    <div class="col-md-8">
      <h2 class="text-left text-primary">VueValidation Form</h2>
      <div
        v-if="submitStatus == 'OK'"
        class="alert alert-success alert-dismissible fade show typo__p"
        role="alert"
      >
        <strong>Success!</strong> form submitted successfully
        <button
          type="button"
          class="close"
          data-dismiss="alert"
          aria-label="Close"
        >
          <span aria-hidden="true">&times;</span>
        </button>
      </div>

      <form @submit.prevent="submit">
        <div class="form-group" :class="{ 'form-group--error': $v.email.$error }">
          <label for="email">Email address</label>
          <input
            type="text"
            class="form-control"
            id="email"
            name="email"
            v-model.trim="email"
            aria-describedby="emailHelp"
            placeholder="Enter email"
          />
        </div>
        <p class="text-danger" v-if="!$v.email.required">Email is required</p>
        <p
          class="text-danger"
          v-if="!$v.email.minLength"
        >Email must have at least {{$v.email.$params.minLength.min}} letters.</p>

        <div class="form-group" :class="{ 'form-group--error': $v.firstname.$error }">
          <label for="fname">First Name</label>
          <input
            type="text"
            class="form-control"
            name="firstname"
            v-model.trim="firstname"
            id="fname"
            placeholder="First Name"
          />
        </div>
        <p class="text-danger" v-if="!$v.firstname.required">Firstname is required</p>
        <p
          class="text-danger"
          v-if="!$v.firstname.minLength"
        >Firstname must have at least {{$v.firstname.$params.minLength.min}} letters.</p>

        <p
          class="text-danger"
          v-if="!$v.firstname.maxLength"
        >Firstname less than 100 {{$v.firstname.$params.maxLength.max}} letters.</p>

        <p class="text-danger" v-if="!$v.firstname.alpha">Firstname must be a letter</p>

        <div class="form-group" :class="{ 'form-group--error': $v.lastname.$error }">
          <label for="lname">Last Name</label>
          <input
            type="text"
            class="form-control"
            v-model.trim="lastname"
            name="lastname"
            id="lname"
            placeholder="Last Name"
          />
        </div>
        <p class="text-danger" v-if="!$v.lastname.required">lastname is required</p>
        <p
          class="text-danger"
          v-if="!$v.lastname.minLength"
        >lastname must have at least {{$v.lastname.$params.minLength.min}} letters.</p>
        <p
          class="text-danger"
          v-if="!$v.lastname.maxLength"
        >lastname must less than 100 {{$v.lastname.$params.maxLength.max}} letters.</p>
        <p class="text-danger" v-if="!$v.lastname.alpha">lastname must be a letter</p>

        <div class="form-group" :class="{ 'form-group--error': $v.username.$error }">
          <label for="lname">Username</label>
          <input
            type="text"
            class="form-control"
            v-model.trim="username"
            name="username"
            id="username"
            placeholder="Username"
          />
        </div>
        <p class="text-danger" v-if="!$v.username.required">Username is required</p>
        <p
          class="text-danger"
          v-if="!$v.username.minLength"
        >Username must have at least {{$v.username.$params.minLength.min}} letters.</p>
        <p
          class="text-danger"
          v-if="!$v.username.maxLength"
        >Username must have at least {{$v.username.$params.maxLength.max}} letters.</p>
        <p class="text-danger" v-if="!$v.username.alpha">Username must be a letter</p>

        <div class="form-group" :class="{ 'form-group--error': $v.password.$error }">
          <label for="password">Password</label>
          <input
            type="password"
            name="password"
            v-model.trim="password"
            class="form-control"
            id="password"
            placeholder="Password"
          />
        </div>
        <p class="text-danger" v-if="!$v.password.required">password is required</p>
        <p
          class="text-danger"
          v-if="!$v.password.minLength"
        >password must have at least {{$v.password.$params.minLength.min}} letters.</p>

        <div class="form-group" :class="{ 'form-group--error': $v.comfirm_password.$error }">
          <label for="Cpassword">Confirm Password</label>
          <input
            type="password"
            name="Cpassword"
            class="form-control"
            v-model.trim="comfirm_password"
            id="Cpassword"
            placeholder="Confirm Password"
          />
        </div>
        <p class="text-danger" v-if="!$v.comfirm_password.required">Confirmation field is required</p>
        <!-- <p
          class="text-danger"
          v-if="!$v.comfirm_password.minLength"
        >commfirmation field must have at least {{$v.comfirm_password.$params.minLength.min}} letters.</p>-->
        <p
          class="text-danger"
          v-if="!$v.comfirm_password.sameAsPassword"
        >Passwords must be identical {{$v.comfirm_password.sameAs }} letters.</p>

        <button
          type="submit"
          :disabled="submitStatus === 'PENDING'"
          id="submit"
          name="submit"
          class="btn btn-primary"
        >Submit</button>
        <button type="reset" name="reset" id="reset" class="btn btn-danger ml-2">Reset</button>
      </form>
    </div>
    <div class="col-md-2"></div>
  </div>
</template>

<script>
const {
  required,
  minLength,
  maxLength,
  sameAs
} = require("vuelidate/lib/validators");
import { helpers } from "vuelidate/lib/validators";
const alpha = helpers.regex("alpha", /^[a-zA-Z]*$/);

export default {
  data() {
    return {
      submitStatus: null,
      email: "",
      firstname: "",
      lastname: "",
      username: "",
      password: "",
      comfirm_password: ""
    };
  },
  validations: {
    firstname: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100),
      alpha
    },
    lastname: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100),
      alpha
    },
    username: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100),
      alpha
    },
    email: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100)
    },
    password: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100)
    },
    comfirm_password: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100),
      sameAsPassword: sameAs(function() {
        return this.password;
      })
    }
  },
  methods: {
    submit() {
      console.log("submit!");
      this.$v.$touch();
      if (this.$v.$invalid) {
        this.submitStatus = "ERROR";
      } else {
        // do your submit logic here
        console.log(
          this.firstname +
            this.lastname +
            this.email +
            this.password +
            this.username
        );
        this.submitStatus = "PENDING";
        setTimeout(() => {
          this.submitStatus = "OK";
        }, 500);
      }
    }
  }
};
</script>

