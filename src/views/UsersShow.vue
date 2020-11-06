<template>
  <div class="users-show">
    <h1>My Account</h1>

    <!-- <img src="" alt="" /> -->
    <h2>{{ user.first_name + ` ${user.last_name}` }}</h2>
    <img :src="user.image" />
    <br />
    <h3>Contact Information</h3>
    <p>Email: {{ user.email }}</p>
    <br />
    <p>Phone Number: {{ user.phone_number }}</p>
    <br />
    <h3>My Events</h3>
    <p>{{ user.events }}</p>
    <br />
    <h3>Attending Events</h3>
    <p>{{ user.attending_events }}</p>
    <br />
    <router-link
      v-if="user.id == $parent.getUserId()"
      :to="`/users/${user.id}/edit`"
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
