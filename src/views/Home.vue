<template>
  <div class="home">
    <!-- HERO BLOCK
    ============================== -->
    <div class="hero hero__bs">
      <div class="hero__inner">
        <div class="wrapper">
          <div class="wrapper__inner">
            <div class="container">
              <div class="row" id="hero__content">
                <div class="col-sm-12 text-center">
                  <h1 class="hero__h1">Welcome to Stake Chicago</h1>
                  <h2 class="hero__h2">Volunteer Platform for Chicagoans</h2>
                  <p class="hero__intro">
                    Stake was made with the desire to build and empower Chicago
                    communities through volunteering.
                  </p>
                  <div class="hero__action">
                    <a href="/signup" class="btn btn-lg btn-primary"
                      >Volunteer Now</a
                    >
                  </div>
                </div>
              </div>
              <!-- / .row -->
            </div>
            <!-- / .container -->
          </div>
          <!-- / .wrapper__inner -->
        </div>
        <!-- / .wrapper -->
      </div>
      <!-- / .hero__inner -->

      <!-- Backstretch controls -->
      <div class="hero__ctrl js-hero-bs__ctrl">
        <a href="#hero__carousel" role="button" data-slide="prev">
          <i class="fa fa-step-backward"></i>
        </a>
        <a href="#" data-slide="pause" class="visible-inline-block playback">
          <i class="fa fa-pause"></i>
        </a>
        <a href="#" data-slide="resume" class="playback hidden">
          <i class="fa fa-play"></i>
        </a>
        <a href="#hero__carousel" role="button" data-slide="next">
          <i class="fa fa-step-forward"></i>
        </a>
      </div>
      <!-- / .hero__ctrl -->
    </div>
    <!-- / .hero -->
    <!-- KEY FEATURES
    ============================== -->
    <div class="section section_features">
      <div class="container">
        <div class="row">
          <div class="col-xs-12">
            <h1 class="section__title">
              Our Core Features
            </h1>
            <p class="section__intro">
              Beatrix is a super flexible HTML5 template that would perfectly
              fit a project of any size and complexity
            </p>
            <div class="section__hr section__hr_center"></div>
          </div>
        </div>
        <!-- / .row -->
        <div class="row">
          <div class="col-xs-12 col-sm-4">
            <!-- Features item #1 -->
            <div class="features__item">
              <!-- Icon -->
              <div class="features__icon wow flipInY">
                <i class="fa fa-check"></i>
                <i class="fa fa-check"></i>
                <!-- Required for animation -->
              </div>
              <!-- Title -->
              <h2 class="features__title">
                Truly Responsive
              </h2>
              <!-- Intro -->
              <p class="features__intro">
                Looks equally great on your laptop, tablet, and phone. Try to
                resize your window to see what we mean.
              </p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-4">
            <!-- Features item #2 -->
            <div class="features__item">
              <!-- Icon -->
              <div class="features__icon wow flipInY" data-wow-delay=".2s">
                <i class="fa fa-birthday-cake"></i>
                <i class="fa fa-birthday-cake"></i>
                <!-- Required for animation -->
              </div>
              <!-- Title -->
              <h2 class="features__title">
                Built with LESS
              </h2>
              <!-- Intro -->
              <p class="features__intro">
                Build on the source LESS code or quickly get started with
                precompiled CSS if you are a novice developer.
              </p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-4">
            <!-- Features item #3 -->
            <div class="features__item">
              <!-- Icon -->
              <div class="features__icon wow flipInY" data-wow-delay=".4s">
                <i class="fa fa-envelope"></i>
                <i class="fa fa-envelope"></i>
                <!-- Required for animation -->
              </div>
              <!-- Title -->
              <h2 class="features__title">
                PHP Contact Form
              </h2>
              <!-- Intro -->
              <p class="features__intro">
                Beatrix comes with a fully functional PHP contact form with
                built in spam protection powered by reCaptcha.
              </p>
            </div>
          </div>
        </div>
        <!-- / .row -->
      </div>
      <!-- / .container -->
    </div>
    <!-- / .section -->
  </div>
</template>

<style></style>

<script>
export default {
  data: function() {
    return {
      message: "Welcome to Stake",
    };
  },
  created: function() {},
  methods: {},
};
</script>
