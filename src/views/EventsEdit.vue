<template>
  <div class="events-edit">
    <div class="container">
      <h1>Edit Event</h1>
      <button
        class="btn btn-danger"
        style="float:right;"
        v-if="event.user_id == $parent.getUserId()"
        v-on:click="destroyEvent()"
      >
        Delete Event</button
      ><br />
      <form v-on:submit.prevent="updateEvent()">
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Title:</label>
          <input type="text" class="form-control" v-model="event.title" />
        </div>

        <div class="form-group">
          <label for="event-time">Start Time:</label>
          <input
            type="datetime-local"
            id="event-time"
            min="2020-01-01T00:00"
            max="2021-12-31T11:59"
            class="form-control"
            v-model="formattedEventStart"
          />
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
        <input type="submit" class="btn btn-primary" value="Submit" />
      </form>
      <br />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  data: function() {
    return {
      event: {},
      errors: [],
      tags: [],
      checkedTagIds: [],
      formattedEventStart: "",
    };
  },
  created: function() {
    axios.get(`/api/events/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.event = response.data;
      this.formattedEventStart = this.formatDate(
        this.event.event_start
      ).substring(0, 19);
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
        event_start: this.formatDate(this.formattedEventStart),
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
        axios
          .delete(`/api/events/${this.event.id}`)
          .then((response) => {
            console.log("Event Successfully Deleted", response.data);
            this.$router.push("/events");
          })
          .catch((error) => {
            this.errors = error.response.data.errors;
          });
      }
    },
    formatDate: function(date) {
      return moment(date).format();
    },
  },
};
</script>
