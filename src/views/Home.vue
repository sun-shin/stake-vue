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
                    Volunteer Platform for the City of Chicago
                  </h2>
                  <p class="hero__intro" id="text">
                    Stake was made with the desire to build and empower Chicago
                    communities through the act of volunteering.
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
              The collective and individual benefits of volunteering can be
              enormous, offering vital help for worthwhile causes, building
              solidarity between groups of people, as well as endless
              opportunities and benefits for the individual.
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
                Form bonds with members of your community or strangers that have
                similar values. Bonds that can ultimately be life-changing.
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
                Health & Well-Being
              </h2>
              <!-- Intro -->
              <p class="features__intro">
                Studies show that volunteering helps people stay physically and
                mentally active, promoting longer life as well. Notably reduces
                stress levels and risk of depression
              </p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-3">
            <!-- Features item #3 -->
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
                Learn new skills from hands-on experiences and continue to
                develop old ones. Consider the endless amounts of knowledge you
                may gain from interaction, building compassion and understanding
                with each individual you meet.
              </p>
            </div>
          </div>
          <div class="col-xs-12 col-sm-3">
            <!-- Features item #4 -->
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
                Allows individuals to try different things, possibility
                narrowing down the search for a future career or determine
                career goals, a natural networking opportunity, and looks great
                on a resume!
              </p>
            </div>
          </div>
        </div>
        <!-- / .row -->
      </div>
      <!-- / .container -->
    </div>
    <!-- / .section -->
    <!-- Volunteer Quotes
    ============================== -->
    <div class="section section_feedback">
      <div class="container">
        <div class="row">
          <div class="col-xs-12">
            <div class="feedback__icon wow zoomIn">
              <i class="fa fa-quote-right"></i>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-xs-12 col-md-10 col-md-offset-1">
            <div
              id="feedback__carousel"
              class="carousel slide"
              data-ride="carousel"
            >
              <!-- Wrapper for slides -->
              <div class="carousel-inner" role="listbox">
                <!-- First item -->
                <div class="item active">
                  <div class="feedback__item">
                    <div class="feedback__content">
                      We make a living by what we get, but we make a life by
                      what we give.
                    </div>
                    <div class="feedback__author">- Winston Churchill -</div>
                  </div>
                </div>
                <!-- Second item -->
                <div class="item">
                  <div class="feedback__item">
                    <div class="feedback__content">
                      The best way to find yourself is in the service of others.
                    </div>
                    <div class="feedback__author">- Gandhi -</div>
                  </div>
                </div>
                <!-- Third item -->
                <div class="item">
                  <div class="feedback__item">
                    <div class="feedback__content">
                      Never doubt that a small group of thoughtful, committed
                      citizens can change the world; indeed, it’s the only thing
                      that ever has.
                    </div>
                    <div class="feedback__author">- Margaret Mead -</div>
                  </div>
                </div>
              </div>

              <!-- Indicators -->
              <ol class="carousel-indicators">
                <li
                  data-target="#feedback__carousel"
                  data-slide-to="0"
                  class="active"
                ></li>
                <li data-target="#feedback__carousel" data-slide-to="1"></li>
                <li data-target="#feedback__carousel" data-slide-to="2"></li>
              </ol>
            </div>
            <!-- / .feedback__carousel -->
          </div>
        </div>
        <!-- / .row -->
      </div>
      <!-- / .container -->
    </div>
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
