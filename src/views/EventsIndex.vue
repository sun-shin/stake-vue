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
              <li><a href="i/">Home</a></li>
              <li class="active">Volunteer Events</li>
            </ol>
          </div>
        </div>
      </div>
    </div>

    <!-- PAGE CONTENT
    ============================== -->
    <div class="container">
      <div class="row">
        <div class="col-sm-8 col-md-9">
          <div class="blog__items">
            <!-- Pause -->
            <div class="blog__item" v-for="event in orderBy(
              filterBy(events, eventSearch, 'title', 'description', 'tags'),
              'event_start'
            )"
            v-if="futureEvent(event.event_start) == true && event.openings != 0">
            <img
              src="img/general_2.jpg"
              alt="..."
              class="img-responsive blog__img hidden-xs"
            />
            <div class="blog__content">
              <h3 class="blog__title">
                <router-link :to="`/events/${event.id}`">
                  {{ event.title }}
                </router-link>
              </h3>
              <ul class="blog__info">
                <li><time datetime="2015-01-29">January 29, 2015</time></li>
                <li><a href="#">Bootstrap</a></li>
                <li>
                  <a href="#"><i class="fa fa-comments-o"></i> 4</a>
                </li>
              </ul>
              <div class="blog__body">
                Nam ultrices, orci sit amet dignissim dignissim, tellus elit
                consequat dui, eu venenatis urna nisi non est. Aliquam egestas
                pulvinar ornare. Aenean et vulputate lacus. Ut eget purus ut
                ante imperdiet feugiat quis vel elit. Donec imperdiet enim
                quis risus porttitor congue. Vestibulum vel tristique urna.
                Pellentesque nulla leo, laoreet sed luctus eu, dapibus id
                lorem. Pellentesque eu tincidunt odio. Proin imperdiet
                bibendum mauris, ut bibendum odio mollis id.
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
          <h3 class="header header_plain">Categories</h3>
          <div class="list-group">
            <a href="#" class="list-group-item active">
              <span class="badge">14</span> Bootstrap
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">17</span> Coding
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">22</span> Design
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">8</span> Graphics
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">21</span> Mobile
            </a>
            <a href="#" class="list-group-item">
              <span class="badge">10</span> UX Design
            </a>
          </div>
        </div>
      </div>
      <!-- / .row -->
    </div>
    <!-- / .container -->

    <ul>
      <!-- <li class="text-danger" v-for="error in errors">{{ error }}</li>
    </ul>
    <div>
      Search Volunteer Events: <input type="text" v-model="eventSearch" />
    </div>
    <div
      v-for="event in orderBy(
        filterBy(events, eventSearch, 'title', 'description', 'tags'),
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
      <p>Street Address: {{ event.address }}</p>
      <p>Description: {{ event.description }}</p>
      <p>Openings: {{ event.openings }}</p>
      <i>Date Posted: {{ dateCreated(event.created_at) }}</i>
    </div> -->
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
