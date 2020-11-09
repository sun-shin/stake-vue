<template>
  <div class="events-show">
    <h1>Event Info</h1>

    <h2>{{ event.title }}</h2>
    <br />
    <button v-on:click="createEventUsers(event.id)">Add</button>
    <p>Created By: {{ event.created_by }}</p>
    <br />
    <p>Event Start: {{ event.event_start }}</p>
    <br />
    <p>Address: {{ event.address }}</p>
    <br />
    <!-- <p>Tag(s): {{ event.tags }}</p><br> -->
    <p>Attendee Limit: {{ event.attendee_limit }}</p>
    <br />
    <p>Openings: {{ event.openings }}</p>
    <br />
    <h3>Attendees</h3>
    <p>{{ event.attendees }}</p>

    <p>Description: {{ event.description }}</p>
    <br />
    <router-link
      v-if="event.user_id == $parent.getUserId()"
      :to="`/events/${event.id}/edit`"
      >Edit</router-link
    >
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
  methods: {
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
