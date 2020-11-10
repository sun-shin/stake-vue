<template>
  <div class="events-new">
    <h1>Create New Event</h1>

    <form v-on:submit.prevent="createEvent()">
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label>Address:</label>
        <input type="text" class="form-control" v-model="address" />
      </div>
      <div class="form-group">
        <label for="event-time">Event Start:</label>
        <input
          type="datetime-local"
          id="event-time"
          min="2020-01-01T00:00"
          max="2021-12-31T11:59"
          class="form-control"
          v-model="eventStart"
        />
      </div>
      <div class="form-group">
        <label>Duration:</label>
        <input type="text" class="form-control" v-model="duration" />
      </div>
      <div class="form-group">
        <label>Attendee Limit:</label>
        <input type="number" class="form-control" v-model="attendeeLimit" />
      </div>
      <div class="form-group">
        <label>Description:</label>
        <input type="text" class="form-control" v-model="description" />
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
      <span>Checked tag ids: {{ checkedTagIds }}</span
      ><br />

      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      title: "",
      createdBy: "",
      eventStart: "",
      duration: "",
      address: "",
      attendeeLimit: "",
      description: "",
      errors: [],
      tags: [],
      checkedTagIds: [],
    };
  },
  created: function() {
    axios.get("/api/tags").then((response) => {
      this.tags = response.data;
      console.log(response.data);
    });
  },
  methods: {
    createEvent: function() {
      var params = {
        title: this.title,
        created_by: this.createdBy,
        event_start: this.eventStart,
        duration: this.duration,
        address: this.address,
        attendee_limit: this.attendeeLimit,
        description: this.description,
        tag_ids: this.checkedTagIds,
      };
      axios
        .post("/api/events", params)
        .then((response) => {
          this.$router.push("/events");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
