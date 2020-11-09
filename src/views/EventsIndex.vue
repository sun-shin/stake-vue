<template>
  <div class="events-index">
    <ul>
      <li class="text-danger" v-for="error in errors">{{ error }}</li>
    </ul>
    <div>
      Search Volunteer Events: <input type="text" v-model="eventSearch" />
    </div>
    <div
      v-for="event in orderBy(
        filterBy(events, eventSearch, 'title', 'description'),
        'event_start'
      )"
    >
      <router-link :to="`/events/${event.id}`">
        <h2>{{ event.title }}</h2>
      </router-link>
      <button v-on:click="createEventUsers(event.id)">Attend</button>
      <p>Event ID: {{ event.id }}</p>
      <!-- update backend view to return full name -->
      <p>Created By: {{ event.user_id }}</p>
      <p>Event Start: {{ event.event_start }}</p>
      <p>Duration: {{ event.duration }}</p>
      <p>Address: {{ event.address }}</p>
      <p>Description: {{ event.description }}</p>
      <p>{{ event.tags }}</p>
      <p>Date Created: {{ dateCreated(event.created_at) }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
import moment from "moment";
import Multiselect from "vue-multiselect";
export default {
  mixins: [Vue2Filters.mixin],
  // components: {
  //   Multiselect,
  // },
  data: function() {
    return {
      events: [],
      errors: [],
      eventSearch: "",
    };
  },
  created: function() {
    this.indexEvents();
  },
  methods: {
    indexEvents: function() {
      axios.get("/api/events").then((response) => {
        console.log(response.data);
        this.events = response.data;
      });
    },
    dateCreated: function(date) {
      return moment(date).format("LL");
    },

    createEventUsers: function(eventId) {
      var params = {
        event_id: eventId,
      };
      axios
        .post("/api/event_users", params)
        .then((response) => {
          this.$router.push("/event_users");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
