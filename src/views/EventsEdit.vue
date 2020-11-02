<template>
  <div class="events-edit">
    <h1>Edit Event</h1>

    <form v-on:submit.prevent="updateEvent()">
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label>Created By:</label>
        <input type="text" class="form-control" v-model="createdBy" />
      </div>
      <div class="form-group">
        <label>Event Start:</label>
        <input type="text" class="form-control" v-model="eventStart" />
      </div>
      <div class="form-group">
        <label>Address:</label>
        <input type="text" class="form-control" v-model="address" />
      </div>
      <!-- <div class="form-group">
        <label>Tag(s):</label>
        <input type="text" class="form-control" v-model="tags" />
      </div> -->
      <div class="form-group">
        <label>Attendee Limit:</label>
        <input type="number" class="form-control" v-model="attendeeLimit" />
      </div>
      <div class="form-group">
        <label>Description:</label>
        <input type="text" class="form-control" v-model="description" />
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
    <button v-on:click="destroyEvent()">Delete Event</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      event: {},
      errors: [],
    };
  },
  created: function() {
    axios.get(`/api/events/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.event = response.data;
    });
  },
  methods: {
    updateEvent: function() {
      var params = {
        title: this.event.title,
        created_by: this.event.createdBy,
        event_start: this.event.eventStart,
        address: this.event.address,
        // tags: this.event.tags,
        attendee_limit: this.event.attendeeLimit,
        description: this.event.description,
      };
      axios
        .patch(`/api/events/${this.event.id}`, params)
        .then((response) => {
          this.$router.push(`/events/${this.event.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyEvent: function() {
      if (confirm("Are you sure you want to delete this event?")) {
        axios.delete(`/api/events/${this.event.id}`).then((response) => {
          console.log("Event Successfully Deleted", response.data);
          this.$router.push("/events");
        });
      }
    },
  },
};
</script>
