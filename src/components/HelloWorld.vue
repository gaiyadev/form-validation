<template>
  <div class="row mt-5">
    <div class="col-md-2"></div>
    <div class="col-md-8">
      <h2 class="text-left text-primary">VueValidation Form</h2>
      <form @submit.prevent="submit">
        <div class="form-group" :class="{ 'form-group--error': $v.email.$error }">
          <label for="email">Email address</label>
          <input
            type="text"
            class="form-control"
            id="email"
            name="email"
            v-model="email"
            aria-describedby="emailHelp"
            placeholder="Enter email"
          />
        </div>
        <p class="text-error" v-if="!$v.email.required">Field is required</p>
        <p
          class="text-error"
          v-if="!$v.email.minLength"
        >Email must have at least {{$v.email.$params.minLength.min}} letters.</p>

        <div class="form-group">
          <label for="fname">First Name</label>
          <input
            type="text"
            class="form-control"
            name="firstname"
            v-model="firstname"
            id="fname"
            placeholder="First Name"
          />
        </div>
        <div class="form-group">
          <label for="lname">Last Name</label>
          <input
            type="text"
            class="form-control"
            v-model="lastname"
            name="lastname"
            id="lname"
            placeholder="Last Name"
          />
        </div>

        <div class="form-group">
          <label for="lname">Username</label>
          <input
            type="text"
            class="form-control"
            v-model="username"
            name="username"
            id="username"
            placeholder="Uername"
          />
        </div>

        <div class="form-group">
          <label for="password">Password</label>
          <input
            type="password"
            name="password"
            v-model="password"
            class="form-control"
            id="password"
            placeholder="Password"
          />
        </div>
        <div class="form-group">
          <label for="Cpassword">Confirm Password</label>
          <input
            type="password"
            name="Cpassword"
            class="form-control"
            v-model="comfirm_password"
            id="Cpassword"
            placeholder="Confirm Password"
          />
        </div>

        <button
          type="submit"
          :disabled="submitStatus === 'PENDING'"
          id="submit"
          name="submit"
          class="btn btn-primary"
        >Submit</button>
        <button type="reset" name="reset" id="reset" class="btn btn-danger ml-2">Reset</button>
        <p class="typo__p" v-if="submitStatus === 'OK'">Thanks for your submission!</p>
        <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
        <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
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
      maxLength: maxLength(100)
    },
    lastname: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100)
    },
    username: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100)
    },
    email: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100)
    },
    password: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100),
      sameAs: sameAs(function() {
        return this.password2;
      })
      // sameAs: sameAs(this.comfirm_password)
    },
    comfirm_password: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(100)
    }
  },
  methods: {
    // setEmail(value) {
    //   this.email = value;
    //   this.$v.email.$touch();
    // }
    submit() {
      console.log("submit!");
      this.$v.$touch();
      if (this.$v.$invalid) {
        this.submitStatus = "ERROR";
      } else {
        // do your submit logic here
        this.submitStatus = "PENDING";
        setTimeout(() => {
          this.submitStatus = "OK";
        }, 500);
      }
    }
  }
};
</script>

