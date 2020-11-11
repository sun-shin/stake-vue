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
      v-if="futureEvent(event.event_start) == true && event.openings != 0"
    >
      <router-link :to="`/events/${event.id}`">
        <h2>{{ event.title }}</h2>
      </router-link>
      Tags:
      <div v-for="tag in event.tags">
        <p>{{ tag.name }}</p>
      </div>
      <br />
      <button v-if="event.attending" v-on:click="destroyEventUser(event)">
        Unattend
      </button>
      <button v-else v-on:click="createEventUser(event)">Attend</button>
      <p>Event ID: {{ event.id }}</p>
      <p>Created By: {{ event.host.first_name }} {{ event.host.last_name }}</p>
      <p>Start Time: {{ $parent.formatDate(event.event_start) }}</p>
      <p>End Time: {{ $parent.eventEnd(event) }}</p>
      <p>Duration: {{ event.duration }}</p>
      <p>Address: {{ event.address }}</p>
      <p>Description: {{ event.description }}</p>
      <p>Openings: {{ event.openings }}</p>
      <i>Date Created: {{ dateCreated(event.created_at) }}</i>
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
      axios.get("/api/events").then((response) => {
        console.log(response.data);
        this.events = response.data;
      });
    },
    dateCreated: function(date) {
      return moment(date).format("LLL");
    },

    createEventUser: function(event) {
      var params = {
        event_id: event.id,
      };
      axios.post("/api/event_users", params).then((response) => {
        event.attendees.push(response.data.user);
        event.attending = true;
        event.openings--;
      });
    },
    destroyEventUser: function(event) {
      var params = {
        event_id: event.id,
      };
      axios.delete(`/api/event_users/${event.id}`).then((response) => {
        event.attending = false;
        var user = event.attendees.find(
          (user) => user.id === response.data.user_id
        );
        var index = event.attendees.indexOf(user);
        event.attendees.splice(index, 1);
        event.openings++;
      });
    },
    futureEvent: function(eventDate) {
      var today = moment().format();
      var eventStart = moment(eventDate).format();
      return today < eventStart;
    },
  },
};
</script>
