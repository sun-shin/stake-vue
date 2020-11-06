<template>
  <div class="events-index">
    <ul>
      <li class="text-danger" v-for="error in errors">{{ error }}</li>
    </ul>
    <div>
      Search Volunteer Events: <input type="text" v-model="eventSearch" />
    </div>
    <div v-for="event in filterBy(events, eventSearch, 'title', 'description')">
      <h2>{{ event.title }}</h2>
      <button v-on:click="createEventUsers()">Add</button>
      <p>Event ID: {{ event.id }}</p>
      <p>{{ $parent.getUserId() }}</p>
      <p>Created By: {{ event.created_by }}</p>
      <p>Event Start: {{ event.event_start }}</p>
      <p>Duration: {{ event.duration }}</p>
      <p>Address: {{ event.address }}</p>
      <p>Description: {{ event.description }}</p>
      <p>{{ event.tags }}</p>
      <router-link :to="`/events/${event.id}`">More Info</router-link>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
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
