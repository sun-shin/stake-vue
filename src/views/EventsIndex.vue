<template>
  <div class="events-index">
    <ul>
      <li class="text-danger" v-for="error in errors">{{ error }}</li>
    </ul>
    <div>
      Search Volunteer Events: <input type="text" v-model="eventSearch" />
    </div>
    <div v-for="event in filterBy(events, eventSearch, 'title', 'description')">
      <router-link :to="`/events/${event.id}`">
        <h2>{{ event.title }}</h2>
      </router-link>
      <button v-on:click="createEventUsers()">Add</button>
      <p>Event ID: {{ event.id }}</p>
      <p>{{ $parent.getUserId() }}</p>
      <p>Created By: {{ event.created_by }}</p>
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
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      events: [],
      errors: [],
      eventSearch: "",
      // eventId: this.event.id,
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
      return moment().format("LL");
    },
    // createEventUsers: function() {
    //   var params = {
    //     user_id: this.$parent.getUserId(),
    //     // event_id: this.eventId,
    //   };
    //   axios
    //     .post("/api/event_users")
    //     .then((response) => {
    //       console.log("You are now attending this event", response.data);
    //       this.$router.push("/event_users");
    //     })
    //     .catch((error) => {
    //       this.errors = error.response.data.errors;
    //     });
    // },
  },
};
</script>
