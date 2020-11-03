<template>
  <div class="events-index">
    <div v-for="event in events">
      <h2>{{ event.title }}</h2>
      <p>Event ID: {{ event.id }}</p>
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
export default {
  data: function() {
    return {
      events: [],
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
  },
};
</script>
