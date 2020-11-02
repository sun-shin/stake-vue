<template>
  <div class="events-show">
    <h1>Event Info</h1>

    <h2>Title: {{ event.title }}</h2>
    <br />
    <p>Created By: {{ event.createdBy }}</p>
    <br />
    <p>Event Start: {{ event.eventStart }}</p>
    <br />
    <p>Address: {{ event.address }}</p>
    <br />
    <!-- <p>Tag(s): {{ event.tags }}</p><br> -->
    <p>Attendee Limit: {{ event.attendeLimit }}</p>
    <br />
    <p>Description: {{ event.description }}</p>
    <br />
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      event: {},
    };
  },
  created: function() {
    console.log(this.$route.params.id);
    axios.get(`/api/events/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.event = response.data;
    });
  },
  methods: {},
};
</script>
