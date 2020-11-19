<template>
  <div class="events-show">
    <!-- PAGE HEADER
    ============================== -->
    <div class="page__header">
      <div class="container">
        <div class="row">
          <div class="col-xs-12">
            <h3 class="page-header__title">Event Info</h3>
            <ol class="breadcrumb page-header__breadcrumb">
              <li><a href="/">Home</a></li>
              <li><a href="/events">Volunteer Events</a></li>
              <li class="active">Event Info</li>
            </ol>
          </div>
        </div>
      </div>
    </div>
    <!-- PAGE CONTENT
    ============================== -->
    <div class="container">
      <div class="row">
        <div class="col-sm-8">
          <div class="portfolio-item__img">
            <!-- Mapbox -->
            <div id="map"></div>
          </div>
        </div>
        <div class="col-sm-4">
          <h3 class="header">
            {{ event.title }}
            <router-link
              class="float-right"
              v-if="event.user_id == $parent.getUserId()"
              :to="`/events/${event.id}/edit`"
            >
              <i class="fa fa-pencil"></i>
            </router-link>
            <p class="text-muted" id="posted">
              Posted {{ $parent.dateCreated(event.created_at) }}
            </p>

            <div v-for="tag in event.tags" id="tags">
              <span class="label label-pill label-warning">{{ tag.name }}</span>
            </div>
          </h3>
          <p class="text-muted">
            {{ event.description }}
          </p>
          <h3 class="header header_plain">Event Details</h3>
          <div class="table-responsive">
            <table class="table">
              <tbody>
                <tr>
                  <th scope="row">Host</th>
                  <td>{{ event.host.first_name }}</td>
                </tr>
                <tr>
                  <th scope="row">Start Time</th>
                  <td>{{ $parent.formatDate(event.event_start) }}</td>
                </tr>
                <tr>
                  <th scope="row">End Time</th>
                  <td>{{ $parent.eventEnd(event) }}</td>
                </tr>
                <tr>
                  <th scope="row">Location</th>
                  <td>{{ event.address }}</td>
                </tr>
                <tr>
                  <th scope="row">Attendee Slots</th>
                  <td>{{ event.openings }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <button
            class="btn btn-primary btn-lg"
            v-if="event.attending"
            v-on:click="destroyEventUser(event)"
          >
            Unattend
          </button>
          <button
            class="btn btn-primary btn-lg"
            v-else
            v-on:click="createEventUser(event)"
          >
            Attend
          </button>
          <!-- / .table-responsive -->
        </div>
      </div>
      <!-- / .row -->
      <div class="row">
        <div class="col-xs-12">
          <h3 class="header">Attendees</h3>

          <!-- Portfolio Carousel -->
          <div
            id="portfolio__carousel"
            class="carousel slide"
            data-ride="carousel"
          >
            <!-- Controls -->
            <div class="portfolio-carousel__controls">
              <a href="#portfolio__carousel" role="button" data-slide="prev">
                <i class="fa fa-long-arrow-left"></i>
              </a>
              <a href="#portfolio__carousel" role="button" data-slide="next">
                <i class="fa fa-long-arrow-right"></i>
              </a>
            </div>

            <!-- Wrapper for slides -->
            <div class="carousel-inner" role="listbox">
              <!-- Slide #1 -->
              <div class="item active">
                <div class="row">
                  <div
                    v-for="attendee in event.attendees"
                    class="col-xs-6 col-sm-6 col-md-3"
                  >
                    <!-- Portfolio Item #1 -->
                    <div class="portfolio__item">
                      <!-- Image -->
                      <div class="portfolio__img">
                        <a href="#">
                          <img :src="attendee.image" alt="Portfolio Image" />
                        </a>
                      </div>
                      <!-- Captions -->
                      <div class="portfolio__caption">
                        <h3 class="portfolio__title">
                          <router-link :to="`/users/${attendee.id}`"
                            >{{ attendee.first_name }}
                            {{ attendee.last_name[0] }}.</router-link
                          >
                        </h3>
                      </div>
                    </div>
                  </div>
                </div>
                <!-- / .row -->
              </div>
              <!-- / .item -->
            </div>
            <!-- / .carousel-inner -->
          </div>
          <!-- / .carousel -->
        </div>
      </div>
    </div>
    <br />
    <!-- / .container -->
  </div>
</template>

<style>
#map {
  width: inherit;
  height: 500px;
}
#posted {
  float: right;
  font-weight: normal;
}
</style>

<script>
/* global MapboxDirections, mapboxSdk*/
import axios from "axios";
import moment from "moment";
import mapboxgl from "mapbox-gl";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";

export default {
  data: function() {
    return {
      event: {
        host: {},
      },
    };
  },
  created: function() {
    console.log(this.$route.params.id);
    axios.get(`/api/events/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.event = response.data;
    });
  },
  mounted: function() {
    this.setMap();
  },
  methods: {
    setMap: function() {
      mapboxgl.accessToken = process.env.VUE_APP_MAP_BOX_KEY;
      var mapboxClient = mapboxSdk({ accessToken: mapboxgl.accessToken });
      var map = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/mapbox/streets-v11",
        center: [-87.62, 41.87],
        zoom: 11,
      });

      map.addControl(
        new MapboxDirections({
          accessToken: mapboxgl.accessToken,
        }),
        "top-left"
      );
      // //
      // mapboxClient.geocoding
      //   .forwardGeocode({
      //     query: event.address,
      //     autocomplete: false,
      //     limit: 1,
      //   })
      //   .send()
      //   .then(function(response) {
      //     if (
      //       response &&
      //       response.body &&
      //       response.body.features &&
      //       response.body.features.length
      //     ) {
      //       var feature = response.body.features[0];
      //       //
      //       var popup = new mapboxgl.Popup({ offset: 25 }).setText(event.title);
      //       var el = document.createElement("div");
      //       //
      //       el.className = "marker";
      //       new mapboxgl.Marker()
      //         .setLngLat(feature.center)
      //         .setPopup(popup)
      //         .addTo(map);
      //     }
      //   });
    },
    createEventUser: function() {
      var params = {
        event_id: this.event.id,
      };
      axios
        .post("/api/event_users", params)
        .then((response) => {
          // puts user into attendees array
          this.event.attendees.push(response.data.user);
          // changes attending to true so button says "unattend"
          this.event.attending = true;
          // decreases openings by one when the user attends
          this.event.openings--;
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyEventUser: function() {
      axios.delete(`/api/event_users/${this.event.id}`).then((response) => {
        // changes attending to false so button says "attend" again
        this.event.attending = false;
        // finds the user in the event attendees array
        var user = this.event.attendees.find(
          (user) => user.id === response.data.user_id
        );
        // find the index of above user in attendees array
        var index = this.event.attendees.indexOf(user);
        // removes attendee from array based on index
        this.event.attendees.splice(index, 1);
        // increases openings by one when the user unattends
        this.event.openings++;
      });
    },
    destroyEvent: function() {
      axios.delete(`/api/events/${this.event.id}`).then((response) => {
        console.log("Success", response.data);
        this.$router.push("/events");
      });
    },
  },
};
</script>
