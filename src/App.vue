<template>
  <div id="app">
    <!-- STATIC TOPBAR
    ============================== -->
    <div class="topbar hidden-xs hidden-sm">
      <div class="container">
        <ul class="topbar-nav topbar-nav_right">
          <!-- Account links -->
          <li v-if="!loggedIn()">
            <router-link to="/login">Login</router-link>
          </li>
          <li v-if="!loggedIn()">
            <router-link to="/signup">Signup</router-link>
          </li>
          <li v-if="loggedIn()">
            <router-link to="/logout">Sign Out</router-link>
          </li>
        </ul>
      </div>
      <!-- / .container -->
    </div>
    <!-- / .topbar -->
    <div class="navbar__hr hidden-xs hidden-sm"></div>
    <!-- STATIC NAVBAR
    ============================== -->
    <div class="navbar navbar-default navbar-static-top" role="navigation">
      <div class="container">
        <div class="navbar-header">
          <!-- Toggle button -->
          <button
            type="button"
            class="navbar-toggle collapsed"
            data-toggle="collapse"
            data-target=".navbar-collapse"
          >
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <!-- Brand name -->
          <a class="navbar-brand" href="/">StakeChicago</a>
        </div>
        <div class="collapse navbar-collapse">
          <!-- Navbar links -->
          <ul class="nav navbar-nav navbar-right">
            <!-- UI Elements -->
            <li><a href="/">Home</a></li>
            <li><router-link to="/events">Volunteer Events</router-link></li>
            <li><router-link to="/events/new">Create Event</router-link></li>
            <li v-if="loggedIn()">
              <router-link :to="`/users/${getUserId()}`"
                >My Profile</router-link
              >
            </li>
          </ul>
        </div>
        <!-- / .navbar-collapse -->
      </div>
      <!-- / .container -->
    </div>
    <div
      v-if="flashMessage"
      class="alert alert-warning alert-dismissible fade show"
      role="alert"
    >
      <strong>{{ flashMessage }}</strong>
      <button
        type="button"
        class="close"
        data-dismiss="alert"
        aria-label="Close"
      >
        <span aria-hidden="true">&times;</span>
      </button>
    </div>
    <router-view />
  </div>
</template>

<script>
import moment from "moment";
export default {
  data: function() {
    return {
      flashMessage: "",
    };
  },
  created: function() {},

  methods: {
    loggedIn: function() {
      return localStorage.getItem("jwt");
    },
    getUserId: function() {
      return localStorage.getItem("user_id");
    },
    formatDate: function(date) {
      return moment(date).format("LLL");
    },
    eventEnd: function(event) {
      var eventStart = event.event_start;
      var calculatedEventEnd = moment(eventStart)
        .add(event.duration, "hours")
        .format("LLL");
      return calculatedEventEnd;
    },
  },
};
</script>
