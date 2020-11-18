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
                  <h1 class="hero__h1" id="text">Welcome to Stake Chicago</h1>
                  <h2 class="hero__h2" id="text">
                    Volunteer Platform for Chicago
                  </h2>
                  <p class="hero__intro" id="text">
                    Stake was made with the desire to build and empower Chicago
                    communities through volunteering.
                  </p>
                  <div class="hero__action">
                    <a href="/signup" class="btn btn-lg btn-primary"
                      >Start Volunteering</a
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
    </div>
    <!-- / .hero -->
    <!-- KEY FEATURES
    ============================== -->
    <div class="section section_features">
      <div class="container">
        <div class="row">
          <div class="col-xs-12">
            <h1 class="section__title">
              Why Volunteer?
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
          <div class="col-xs-12 col-sm-3">
            <!-- Features item #1 -->
            <div class="features__item">
              <!-- Icon -->
              <div class="features__icon wow flipInY">
                <i class="fa fa-leaf"></i>
                <i class="fa fa-leaf"></i>
                <!-- Required for animation -->
              </div>
              <!-- Title -->
              <h2 class="features__title">
                Growth
              </h2>
              <!-- Intro -->
              <p class="features__intro">
                Looks equally great on your laptop, tablet, and phone. Try to
                resize your window to see what we mean.
              </p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-3">
            <!-- Features item #2 -->
            <div class="features__item">
              <!-- Icon -->
              <div class="features__icon wow flipInY" data-wow-delay=".2s">
                <i class="fa fa-heartbeat"></i>
                <i class="fa fa-heartbeat"></i>
                <!-- Required for animation -->
              </div>
              <!-- Title -->
              <h2 class="features__title">
                Health
              </h2>
              <!-- Intro -->
              <p class="features__intro">
                Build on the source LESS code or quickly get started with
                precompiled CSS if you are a novice developer.
              </p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-3">
            <!-- Features item #3 -->
            <div class="features__item">
              <!-- Icon -->
              <div class="features__icon wow flipInY" data-wow-delay=".4s">
                <i class="fa fa-group"></i>
                <i class="fa fa-group"></i>
                <!-- Required for animation -->
              </div>
              <!-- Title -->
              <h2 class="features__title">
                Connections
              </h2>
              <!-- Intro -->
              <p class="features__intro">
                Beatrix comes with a fully functional PHP contact form with
                built in spam protection powered by reCaptcha.
              </p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-3">
            <!-- Features item #3 -->
            <div class="features__item">
              <!-- Icon -->
              <div class="features__icon wow flipInY" data-wow-delay=".4s">
                <i class="fa fa-briefcase"></i>
                <i class="fa fa-briefcase"></i>
                <!-- Required for animation -->
              </div>
              <!-- Title -->
              <h2 class="features__title">
                Career
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

<style>
/* #wrapper {
  opacity: 0.5;
} */
#text {
  text-shadow: 2px 2px #000000;
}
</style>

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
