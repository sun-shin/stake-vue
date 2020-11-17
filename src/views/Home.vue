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
                  <h2 class="hero__h2">Easily With Beatrix</h2>
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
