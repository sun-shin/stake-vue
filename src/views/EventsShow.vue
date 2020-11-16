<template>
  <div class="events-show">
    <h1>Event Info</h1>

    <h2>{{ event.title }}</h2>
    <br />
    Tags:
    <div v-for="tag in event.tags">
      <p>{{ tag.name }}</p>
    </div>
    <br />
    <button v-if="event.attending" v-on:click="destroyEventUser()">
      Unattend
    </button>
    <button v-else v-on:click="createEventUser()">Attend</button>

    <p>Created By: {{ event.host.first_name }} {{ event.host.last_name }}</p>
    <br />
    <p>Start Time: {{ $parent.formatDate(event.event_start) }}</p>
    <br />
    <p>End Time: {{ $parent.eventEnd(event) }}</p>
    <br />
    <p>Street Address: {{ event.address }}</p>
    <br />
    <p>Description: {{ event.description }}</p>
    <br />
    <p>Attendee Limit: {{ event.attendee_limit }}</p>
    <br />
    <p>Openings: {{ event.openings }}</p>
    <br />

    <h3>Attendees</h3>
    <!-- put in modal after installing theme -->
    <div v-for="attendee in event.attendees">
      <router-link :to="`/users/${attendee.id}`">
        <p>{{ attendee.first_name }} {{ attendee.last_name }}</p>
      </router-link>
    </div>
    <br />
    <router-link
      v-if="event.user_id == $parent.getUserId()"
      :to="`/events/${event.id}/edit`"
      >Edit</router-link
    >
    <br />
    <div id="map"></div>
  </div>
</template>

<style>
#map {
  width: 50vw;
  height: 50vh;
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
