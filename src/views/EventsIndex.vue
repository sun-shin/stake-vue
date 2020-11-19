<template>
  <div class="events-index">
    <!-- PAGE HEADER
    ============================== -->
    <div class="page__header">
      <div class="container">
        <div class="row">
          <div class="col-xs-12">
            <h3 class="page-header__title">Volunteer Events</h3>
            <ol class="breadcrumb page-header__breadcrumb">
              <li>
                <div>
                  Search Volunteer Events:
                  <input
                    type="text"
                    style="height:25px"
                    v-model="eventSearch"
                  />
                </div>
              </li>
            </ol>
          </div>
        </div>
      </div>
    </div>
    <!-- PAGE CONTENT
    ============================== -->
    <div class="container">
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="row">
        <div class="col-sm-8 col-md-9">
          <div class="blog__items">
            <div
              class="blog__item"
              v-for="event in orderBy(
                filterBy(events, eventSearch, 'title', 'description', 'tags'),
                'event_start'
              )"
              v-if="
                futureEvent(event.event_start) == true && event.openings != 0
              "
            >
              <div class="blog__content">
                <h3 class="blog__title">
                  <router-link :to="`/events/${event.id}`">
                    {{ event.title }}
                  </router-link>
                </h3>
                <ul class="blog__info">
                  <li>
                    <time datetime="2015-01-29">{{
                      $parent.dateCreated(event.created_at)
                    }}</time>
                  </li>
                  <li>
                    <p>
                      Host: {{ event.host.first_name }}
                      {{ event.host.last_name[0] }}.
                    </p>
                  </li>
                  <li>
                    <button
                      v-if="event.attending"
                      v-on:click="destroyEventUser(event)"
                      class="btn btn-primary btn-xs"
                    >
                      Unattend
                    </button>
                    <button
                      v-else
                      v-on:click="createEventUser(event)"
                      class="btn btn-primary btn-xs"
                    >
                      Attend
                    </button>
                  </li>
                </ul>
                <div class="blog__body">
                  <p>Start: {{ $parent.formatDate(event.event_start) }}</p>
                  <p>End: {{ $parent.eventEnd(event) }}</p>
                  <p>Description: {{ event.description }}</p>
                  <p>Openings: {{ event.openings }}</p>

                  <div v-for="tag in event.tags" id="tags">
                    <span class="label label-pill label-warning">{{
                      tag.name
                    }}</span>
                  </div>
                </div>
              </div>
            </div>
            <!-- / .blog__item -->
          </div>
          <!-- / .blog__items -->
          <!-- Pager -->
          <ul class="pager pull-right">
            <li class="disabled"><a href="#">Previous</a></li>
            <li><a href="#">Next</a></li>
          </ul>
          <div class="clearfix"></div>
        </div>
        <div class="col-sm-4 col-md-3">
          <!-- Categories -->
          <h3 class="header header_plain">Event Tags</h3>
          <div class="list-group">
            <a href="#" class="list-group-item">
              <span class="badge">2</span> Community
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">2</span> Sports
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">2</span> Health
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">1</span> Arts & Culture
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">1</span> Animals
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">1</span> Environment
            </a>
          </div>
        </div>
      </div>
      <!-- / .row -->
    </div>
    <!-- / .container -->
  </div>
</template>

<style>
#tags {
  display: inline;
  padding: 2.5px;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
import moment from "moment";
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      events: [],
      eventSearch: "",
      errors: [],
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
    createEventUser: function(event) {
      var params = {
        event_id: event.id,
      };
      axios
        .post("/api/event_users", params)
        .then((response) => {
          event.attendees.push(response.data.user);
          event.attending = true;
          event.openings--;
        })
        .catch((error) => {
          this.errors = ["Please Login or Signup to Attend"];
          // this.$parent.flashMessage = "Please Login or Signup to Attend";
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
