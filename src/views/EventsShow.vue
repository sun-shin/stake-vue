<template>
  <div class="events-show">
    <h1>Event Info</h1>

    <h2>{{ event.title }}</h2>
    <br />
    <button v-if="event.attending" v-on:click="destroyEventUser()">
      Unattend
    </button>
    <button v-else v-on:click="createEventUser()">Attend</button>

    <p>Created By: {{ event.host.first_name }} {{ event.host.last_name }}</p>
    <br />
    <p>Event Start: {{ formatDate(event.event_start) }}</p>
    <br />
    <p>Address: {{ event.address }}</p>
    <br />
    <p>Description: {{ event.description }}</p>
    <br />
    <p>Attendee Limit: {{ event.attendee_limit }}</p>
    <br />
    <p>Openings: {{ event.openings }}</p>
    <br />
    <h3>Attendees</h3>
    <div v-for="attendee in event.attendees">
      <p>Name: {{ attendee.first_name }} {{ attendee.last_name }}</p>
    </div>
    <br />
    <router-link
      v-if="event.user_id == $parent.getUserId()"
      :to="`/events/${event.id}/edit`"
      >Edit</router-link
    >
    <br />
    <button
      class="btn btn-danger"
      v-if="event.user_id == $parent.getUserId()"
      v-on:click="destroyEvent()"
    >
      Delete Event
    </button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import moment from "moment";
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
    formatDate: function(date) {
      return moment(date).format("LL");
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
