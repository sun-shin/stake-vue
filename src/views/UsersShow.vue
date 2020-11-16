<template>
  <div class="users-show">
    <h1>{{ user.first_name }} {{ user.last_name }}</h1>
    <!-- <div v-if="hoursCompleted() >= 100">
      <img src="/images/HandIcon.png" id="icon" />
    </div> -->
    <img :src="user.image" />
    <br />
    <h2>Contact Information</h2>
    <p>Email: {{ user.email }}</p>

    <p>Phone Number: {{ user.phone_number }}</p>

    Hours Completed:

    <!-- <p>{{ hoursCompleted() }}</p> -->
    <br />

    <router-link
      v-if="user.id == $parent.getUserId()"
      :to="`/users/${user.id}/edit`"
      >Edit </router-link
    ><br />

    <h2>Attending Events</h2>
    <div
      v-for="event in user.attending_events"
      v-if="futureEvent(event.event_start) == true"
    >
      <router-link :to="`/events/${event.id}`">
        <h3>{{ event.title }}</h3>
      </router-link>
      <p>Start Time: {{ $parent.formatDate(event.event_start) }}</p>
      <p>End Time: {{ $parent.eventEnd(event) }}</p>
      <p>{{ event.tags }}</p>
      <!-- <p>Future Event?: {{ futureEvent(event.event_start) }}</p> -->
    </div>

    <h2>My Events</h2>
    <div
      v-for="event in user.events"
      v-if="futureEvent(event.event_start) == true"
    >
      <router-link :to="`/events/${event.id}`">
        <h3>{{ event.title }}</h3>
      </router-link>
      <p>Start Time: {{ $parent.formatDate(event.event_start) }}</p>
    </div>
    <br />
  </div>
</template>

<style>
#icon {
  width: 3%;
  height: 3%;
}
</style>

<script>
import axios from "axios";
import moment from "moment";
export default {
  data: function() {
    return {
      user: {},
    };
  },
  created: function() {
    console.log(this.$route.params.id);
    axios.get(`/api/users/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.user = response.data;
    });
  },
  methods: {
    futureEvent: function(eventDate) {
      var today = moment().format();
      var eventStart = moment(eventDate).format();
      return today < eventStart;
    },
    // hoursCompleted: function() {
    //   var completedHours = 0;
    //   var today = moment().format();
    //   var eventStart = moment(event.event_start).format();
    //   this.user.attending_events.forEach(function(event) {
    //     if (today < eventStart === false) {
    //       completedHours += event.duration;
    //     }
    //   });
    //   return completedHours;
    // },
    // showPastEvents: function(event) {
    //   var attendingEvents = this.user.attending_events;
    //   attendingEvents.forEach((event) => {
    //     if (this.futureEvent(event.event_start) == false)
    //   });
    // },
  },
};
</script>
