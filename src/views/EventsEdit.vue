<template>
  <div class="events-edit">
    <h1>Edit Event</h1>

    <form v-on:submit.prevent="updateEvent()">
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="event.title" />
      </div>

      <div class="form-group">
        <label>Event Start:</label>
        <input type="text" class="form-control" v-model="event.event_start" />
      </div>
      <div class="form-group">
        <label>Duration:</label>
        <input type="number" class="form-control" v-model="event.duration" />
      </div>
      <div class="form-group">
        <label>Address:</label>
        <input type="text" class="form-control" v-model="event.address" />
      </div>

      <div class="form-group">
        <label>Attendee Limit:</label>
        <input
          type="number"
          class="form-control"
          v-model="event.attendee_limit"
        />
      </div>
      <div class="form-group">
        <label>Description:</label>
        <input type="text" class="form-control" v-model="event.description" />
      </div>
      <div v-for="tag in tags">
        <input
          type="checkbox"
          :id="tag.id"
          :value="tag.id"
          v-model="checkedTagIds"
        />
        <label :for="tag.id">{{ tag.name }}</label>
      </div>

      <br />
      <span>Checked tag ids: {{ checkedTagIds }}</span>
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
      tags: [],
      checkedTagIds: [],
    };
  },
  created: function() {
    axios.get(`/api/events/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.event = response.data;
      this.checkedTagIds = this.event.tags.map((tag) => tag.id);
    });
    axios.get("/api/tags").then((response) => {
      this.tags = response.data;
      console.log(response.data);
    });
  },
  methods: {
    updateEvent: function() {
      var params = {
        title: this.event.title,
        event_start: this.event.event_start,
        address: this.event.address,
        attendee_limit: this.event.attendee_limit,
        duration: this.event.duration,
        description: this.event.description,
        tag_ids: this.checkedTagIds,
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
