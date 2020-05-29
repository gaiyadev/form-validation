<template>
  <div class="row mt-5">
    <div class="col-md-2"></div>
    <div class="col-md-8">
      <h2 class="text-left text-primary">VeeValidation Form</h2>
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

      <ValidationObserver ref="form">
        <form @submit.prevent="onSubmit">
          <validation-provider rules="required|min:3|email|max:100" v-slot="{errors }">
            <div class="form-group">
              <label for="email">Email address</label>
              <input
                type="text"
                class="form-control"
                id="email"
                name="email"
                v-model.trim="email"
                placeholder="Enter email"
              />
              <span class="text-danger">{{ errors[0] }}</span>
            </div>
          </validation-provider>

          <ValidationProvider rules="required|min:3|alpha|max:100" v-slot="{ errors }">
            <div class="form-group">
              <label for="fname">First Name</label>
              <input
                type="text"
                class="form-control"
                name="firstname"
                v-model.trim="firstname"
                id="fname"
                placeholder="First Name"
              />
              <span class="text-danger">{{ errors[0] }}</span>
            </div>
          </ValidationProvider>

          <validation-provider rules="required|min:3|alpha|max:100" v-slot="{ errors }">
            <div class="form-group">
              <label for="lname">Last Name</label>
              <input
                type="text"
                class="form-control"
                v-model.trim="lastname"
                name="lastname"
                id="lname"
                placeholder="Last Name"
              />
              <span class="text-danger">{{ errors[0] }}</span>
            </div>
          </validation-provider>

          <validation-provider rules="required|min:3|max:100|alpha" v-slot="{ errors }">
            <div class="form-group">
              <label for="lname">Username</label>
              <input
                type="text"
                class="form-control"
                v-model.trim="username"
                name="username"
                id="username"
                placeholder="Username"
              />
              <span class="text-danger">{{ errors[0] }}</span>
            </div>
          </validation-provider>
          <validation-provider
            rules="required|min:3|max:100|alpha_num"
            v-slot="{ errors }"
            vid="password"
          >
            <div class="form-group">
              <label for="password">Password</label>
              <input
                type="password"
                name="password"
                v-model.trim="password"
                class="form-control"
                id="password"
                placeholder="Password"
              />
              <span class="text-danger">{{ errors[0] }}</span>
            </div>
          </validation-provider>

          <validation-provider
            rules="required|min:3|max:100|confirmed:password"
            v-slot="{ errors }"
          >
            <div class="form-group">
              <label for="Cpassword">Confirm Password</label>
              <input
                type="password"
                name="Cpassword"
                class="form-control"
                v-model.trim="comfirm_password"
                id="Cpassword"
                placeholder="Confirm Password"
              />
              <span class="text-danger">{{ errors[0] }}</span>
            </div>
          </validation-provider>

          <button
            type="submit"
            :disabled="submitStatus === 'PENDING'"
            id="submit"
            name="submit"
            class="btn btn-primary"
          >Submit</button>
          <button type="reset" name="reset" id="reset" class="btn btn-danger ml-2">Reset</button>
        </form>
      </ValidationObserver>
    </div>
    <div class="col-md-2"></div>
  </div>
</template>

<script>
import { ValidationProvider, ValidationObserver } from "vee-validate";
import { extend } from "vee-validate";
import {
  required,
  email,
  min,
  max,
  alpha,
  alpha_num,
  confirmed
} from "vee-validate/dist/rules";

extend("email", email);

// Override the default message.
extend("required", {
  ...required,
  message: "Email is required"
});
extend("min", {
  ...min,
  message: "This field must be more than 3"
});

extend("max", {
  ...max,
  message: "This field must be withen the char range"
});
extend("alpha", {
  ...alpha,
  message: "This field is must be alpha"
});
extend("alpha_num", {
  ...alpha_num,
  message: "Password must be alphanum"
});
extend("confirmed", {
  ...confirmed,
  message: "Password confirmation failed"
});

export default {
  components: {
    ValidationProvider,
    ValidationObserver
  },
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

  methods: {
    onSubmit() {
      this.$refs.form.validate().then(success => {
        if (!success) {
          return;
        }

        this.submitStatus = "PENDING";
        setTimeout(() => {
          this.submitStatus = "OK";
        }, 500);

        // Resetting Values
        this.firstname = this.lastname = this.email = this.password = this.comfirm_password = this.username =
          "";

        // Wait until the models are updated in the UI
        this.$nextTick(() => {
          this.$refs.form.reset();
        });
      });
    }
  }
};
</script>

