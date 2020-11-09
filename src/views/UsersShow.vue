<template>
  <div class="users-show">
    <h1>{{ user.first_name + ` ${user.last_name}` }}</h1>
    <img :src="user.image" />
    <br />
    <h2>Contact Information</h2>
    <p>Email: {{ user.email }}</p>

    <p>Phone Number: {{ user.phone_number }}</p>

    <router-link
      v-if="user.id == $parent.getUserId()"
      :to="`/users/${user.id}/edit`"
      >Edit </router-link
    ><br />

    <h2>Attending Events</h2>
    <div v-for="event in user.attending_events">
      <router-link :to="`/events/${event.id}`">
        <h3>{{ event.title }}</h3>
      </router-link>
      <p>{{ event.event_start }}</p>
      <p>Duration: {{ event.duration }}</p>
      <p>{{ event.tags }}</p>
    </div>

    <h2>My Events</h2>
    <div v-for="event in user.events">
      <router-link :to="`/events/${event.id}`">
        <h3>{{ event.title }}</h3>
      </router-link>
      <p>{{ event.event_start }}</p>
      <p>{{ event.duration }}</p>
    </div>
    <br />
  </div>
</template>

<style></style>

<script>
import axios from "axios";
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
  methods: {},
};
</script>
