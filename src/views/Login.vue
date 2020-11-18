<template>
  <div class="login">
    <!-- PAGE CONTENT
    ============================== -->
    <div class="container">
      <div class="row">
        <div class="col-xs-12">
          <div class="body-plain__title">
            <h1 class="text-center">Sign In</h1>
          </div>
        </div>
      </div>
      <!-- / .row -->
      <div class="row">
        <div
          class="col-xs-12 col-sm-6 col-md-6 col-lg-4 col-sm-offset-3 col-md-offset-3 col-lg-offset-4"
        >
          <div class="body-plain__form">
            <form role="form" v-on:submit.prevent="submit()">
              <ul>
                <li class="text-danger" v-for="error in errors">{{ error }}</li>
              </ul>
              <label for="sign-in__email" class="sr-only">Enter email</label>
              <div class="input-group">
                <span class="input-group-addon"
                  ><i class="fa fa-user"></i
                ></span>
                <input
                  type="email"
                  class="form-control"
                  v-model="email"
                  id="sign-in__email"
                  placeholder="Enter email"
                />
              </div>
              <br />
              <label for="sign-in__password" class="sr-only"
                >Enter password</label
              >
              <div class="input-group">
                <span class="input-group-addon"
                  ><i class="fa fa-lock"></i
                ></span>
                <input
                  type="password"
                  class="form-control"
                  v-model="password"
                  id="sign-in__password"
                  placeholder="Password"
                />
              </div>
              <br />

              <input
                type="submit"
                class="btn btn-primary btn-block btn-lg"
                value="Submit"
              />
            </form>

            <!-- Sign Up link -->
            <hr />
            <p>
              Not registered?
              <router-link to="/signup">Create an Account.</router-link>
            </p>
          </div>
          <!-- / .body-plain__form -->
        </div>
      </div>
      <!-- / .row -->
    </div>
    <br />
    <!-- / .container -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      email: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function() {
      var params = {
        email: this.email,
        password: this.password,
        user_id: this.$parent.getUserId(),
      };

      axios
        .post("/api/sessions", params)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] =
            "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          this.$parent.flashMessage = "Login Successful";
          this.$router.push(`/users/${this.$parent.getUserId()}`);
        })
        .catch((error) => {
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
  },
};
</script>
