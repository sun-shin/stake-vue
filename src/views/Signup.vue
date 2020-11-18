<template>
  <div class="signup">
    <!-- PAGE CONTENT
    ============================== -->
    <div class="container">
      <div class="row">
        <div class="col-xs-12">
          <div class="body-plain__title">
            <h1 class="text-center">Sign Up</h1>
          </div>
        </div>
      </div>
      <!-- / .row -->
      <div class="row">
        <div
          class="col-xs-12 col-sm-6 col-md-6 col-lg-4 col-sm-offset-3 col-md-offset-3 col-lg-offset-4"
        >
          <div class="body-plain__form">
            <!-- Sign Up form -->
            <form role="form" v-on:submit.prevent="submit()">
              <div class="form-group">
                <label for="sign-up__first_name" class="sr-only"
                  >First Name</label
                >
                <input
                  type="text"
                  class="form-control"
                  v-model="firstName"
                  id="sign-up__first_name"
                  placeholder="First Name"
                />
              </div>
              <div class="form-group">
                <label for="sign-up__last_name" class="sr-only"
                  >Last Name</label
                >
                <input
                  type="text"
                  class="form-control"
                  v-model="lastName"
                  id="sign-up__last_name"
                  placeholder="Last Name"
                />
              </div>
              <div class="form-group">
                <label for="sign-up__email" class="sr-only">Enter Email</label>
                <input
                  type="email"
                  class="form-control"
                  v-model="email"
                  id="sign-up__email"
                  placeholder="Enter email"
                />
              </div>
              <div class="form-group">
                <label for="sign-up__phone_number" class="sr-only"
                  >Phone Number</label
                >
                <input
                  type="text"
                  class="form-control"
                  v-model="phoneNumber"
                  id="sign-up__phone_number"
                  placeholder="Phone Number"
                />
              </div>
              <div class="form-group">
                <div class="row">
                  <div class="col-sm-6">
                    <div class="form-group">
                      <label for="sign-up__password" class="sr-only"
                        >Enter Password</label
                      >
                      <input
                        type="password"
                        class="form-control"
                        v-model="password"
                        id="sign-up__password"
                        placeholder="Password"
                      />
                    </div>
                  </div>
                  <div class="col-sm-6">
                    <div class="form-group">
                      <label for="sign-up__password_repeat" class="sr-only"
                        >Password Confirmation</label
                      >
                      <input
                        type="password"
                        class="form-control"
                        v-model="passwordConfirmation"
                        id="sign-up__password_repeat"
                        placeholder="Confirm Password"
                      />
                    </div>
                  </div>
                </div>
              </div>
              <button
                type="submit"
                class="btn btn-primary btn-block btn-lg"
                value="Submit"
              >
                Create Account
              </button>
            </form>

            <!-- Sign In link -->
            <hr />
            <p>
              Already a member?
              <router-link to="/login">Sign into your account.</router-link>
            </p>
          </div>
          <!-- / .body-plain__form -->
        </div>
      </div>
      <!-- / .row -->
    </div>
    <!-- / .container -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      firstName: "",
      lastName: "",
      email: "",
      phoneNumber: "",
      password: "",
      passwordConfirmation: "",
      errors: [],
    };
  },
  methods: {
    submit: function() {
      var params = {
        first_name: this.firstName,
        last_name: this.lastName,
        email: this.email,
        phone_number: this.phoneNumber,
        password: this.password,
        password_confirmation: this.passwordConfirmation,
      };
      axios
        .post("/api/users", params)
        .then((response) => {
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
