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
    <!-- FOOTER
    ============================== -->
    <footer>
      <!-- Footer Top -->
      <div class="footer_top hidden-xs">
        <div class="container">
          <div class="row">
            <!-- Contact Us -->
            <div class="col-xs-12 col-sm-4">
              <div class="footer__item">
                <div class="footer__title">Contact Us</div>
                <div class="footer__content">
                  <p>
                    Do not hesitate to contact us if you have any questions or
                    for feedback:
                  </p>
                  <p>Phone: (312) 000-0000<br /></p>
                  <p>Email: <a href="#">support@stake.com</a></p>
                </div>
              </div>
            </div>

            <!-- Recent Posts -->
            <div class="col-xs-12 col-sm-4">
              <div class="footer__item">
                <div class="footer__title">Recent Events</div>
                <div class="footer__content">
                  <div class="footer__post">
                    <div class="footer-post__img">
                      <img src="img/general_1.jpg" alt="..." />
                    </div>
                    <div class="footer-post__content">
                      <p><a href="#">Lorem ipsum dolor sit amet</a></p>
                      <time datetime="2015-01-01">2015/01/01</time>
                    </div>
                  </div>
                  <div class="footer__post">
                    <div class="footer-post__img">
                      <img src="img/general_2.jpg" alt="..." />
                    </div>
                    <div class="footer-post__content">
                      <p><a href="#">Lorem ipsum dolor sit amet</a></p>
                      <time datetime="2015-01-01">2015/01/01</time>
                    </div>
                  </div>
                  <div class="footer__post">
                    <div class="footer-post__img">
                      <img src="img/general_3.jpg" alt="..." />
                    </div>
                    <div class="footer-post__content">
                      <p><a href="#">Lorem ipsum dolor sit amet</a></p>
                      <time datetime="2015-01-01">2015/01/01</time>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <!-- Quick Links -->
            <div class="col-xs-12 col-sm-4">
              <div class="footer__item">
                <div class="footer__title">Quick Links</div>
                <div class="footer__content">
                  <ul class="footer__links">
                    <li><a href="/">Home</a></li>
                    <li>
                      <router-link to="/events">Volunteer Events</router-link>
                    </li>
                    <li>
                      <router-link to="/events/new">Create Event</router-link>
                    </li>
                    <li v-if="loggedIn()">
                      <router-link :to="`/users/${getUserId()}`"
                        >My Profile</router-link
                      >
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
          <!-- / .row -->
        </div>
        <!-- / .container -->
      </div>
      <!-- / .footer_top -->
      <div class="footer__hr hidden-xs"></div>

      <!-- Footer Bottom -->
      <div class="footer_bottom">
        <div class="container">
          <div class="row">
            <div class="col-xs-12 col-sm-6">
              <div class="footer__copyright">
                Copyright 2015
                <a href="http://sun-shin.github.io">Sunhan Shin</a>. All Rights
                Reserved.
              </div>
            </div>
            <div class="col-xs-12 col-sm-6">
              <ul class="footer__social">
                <li class="twitter">
                  <a href="#"><i class="fa fa-twitter"></i></a>
                </li>
                <li class="facebook">
                  <a href="#"><i class="fa fa-facebook"></i></a>
                </li>
                <li class="linkedin">
                  <a href="#"><i class="fa fa-linkedin"></i></a>
                </li>
                <li class="google-plus">
                  <a href="#"><i class="fa fa-google-plus"></i></a>
                </li>
                <li class="pinterest">
                  <a href="#"><i class="fa fa-pinterest"></i></a>
                </li>
              </ul>
            </div>
          </div>
          <!-- / .row -->
        </div>
        <!-- / .container -->
      </div>
      <!-- / .footer_bottom -->
    </footer>
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
    dateCreated: function(date) {
      return moment(date).format("LL");
    },
  },
};
</script>
