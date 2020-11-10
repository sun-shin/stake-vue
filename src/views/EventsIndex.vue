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
      Tags:
      <div v-for="tag in event.tags">
        <p>{{ tag.name }}</p>
      </div>
      <br />
      <!-- <button v-if="event.attending" v-on:click="destroyEventUser(event)">
        Unattend
      </button> -->
      <button v-on:click="createEventUsers(event)">Attend</button>
      <p>Event ID: {{ event.id }}</p>
      <!-- update backend view to return full name -->
      <p>Created By: {{ event.host.first_name }} {{ event.host.last_name }}</p>
      <p>Event Start: {{ formatDate(event.event_start) }}</p>
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
    };
  },
  created: function() {
    this.indexEvents();
  },
  methods: {
    indexEvents: function() {
      axios
        .get("/api/events")
        .then((response) => {
          console.log(response.data);
          this.events = response.data;
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    dateCreated: function(date) {
      return moment(date).format("LL");
    },

    createEventUsers: function(event) {
      var params = {
        event_id: event.id,
      };
      axios.post("/api/event_users", params).then((response) => {
        this.event.attendees.push(response.data.user);
        this.event.attending = true;
        this.event.openings--;
      });
    },
    // destroyEventUser: function(event) {
    //   var params = {
    //     event_id: event.id,
    //   };
    //   axios.delete(`/api/event_users/${this.eventId}`).then((response) => {
    //     this.event.attending = false;
    //     var user = this.event.attendees.find(
    //       (user) => user.id === response.data.user_id
    //     );
    //     var index = this.event.attendees.indexOf(user);
    //     this.event.attendees.splice(index, 1);
    //     this.event.openings++;
    //   });
    // },
    formatDate: function(date) {
      return moment(date).format("LL");
    },
  },
};
</script>
