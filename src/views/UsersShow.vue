<template>
  <div class="users-show">
    <!-- PAGE CONTENT
    ============================== -->
    <div class="profile__header">
      <div class="container">
        <div class="row">
          <div class="col-sm-4 col-md-3 col-lg-2">
            <!-- User avatar -->
            <div class="profile__avatar">
              <img
                :src="user.image"
                alt="..."
                class="img-responsive center-block"
              />
            </div>
          </div>
          <div class="col-sm-8 col-md-9 col-lg-10">
            <div class="profile__summary">
              <!-- User name -->
              <h3 class="profile__name">
                {{ user.first_name }} {{ user.last_name[0] }}.
              </h3>
              <div v-if="hoursCompleted() >= 100">
                <img src="/img/HandIcon.png" id="icon" />
              </div>
              <!-- User status -->
              <p class="text-muted">
                <!-- user bio? -->
              </p>
              <!-- Contact -->
              <router-link
                v-if="user.id == $parent.getUserId()"
                :to="`/users/${user.id}/edit`"
                class="btn btn-primary btn-sm"
              >
                Edit
              </router-link>
              <span v-if="$parent.loggedIn()">
                <a href="/logout" class="btn btn-default btn-sm"
                  ><i class="fa fa-sign-out"></i> Sign Out</a
                >
              </span>
            </div>
            <!-- / .profile__summary -->
          </div>
        </div>
        <!-- / .row -->
      </div>
      <!-- / .container -->
    </div>
    <!-- .profile__header -->
    <div class="container">
      <div class="row">
        <div class="col-sm-4 col-md-3 col-lg-2"></div>
        <div class="col-sm-8 col-md-9 col-lg-10">
          <!-- Tabs -->
          <ul class="nav nav-tabs" role="tablist">
            <li role="presentation" class="active">
              <a
                href="#personal-info"
                aria-controls="personal-info"
                role="tab"
                data-toggle="tab"
                >Personal Info</a
              >
            </li>
            <li role="presentation">
              <a
                href="#my-events"
                aria-controls="my-events"
                role="tab"
                data-toggle="tab"
                >My Events</a
              >
            </li>
            <li role="presentation">
              <a
                href="#attending-events"
                aria-controls="attending-events"
                role="tab"
                data-toggle="tab"
                >Attending Events</a
              >
            </li>
            <li role="presentation">
              <a
                href="#past-events"
                aria-controls="past-events"
                role="tab"
                data-toggle="tab"
                >Past Events</a
              >
            </li>
          </ul>
          <!-- Tab content -->
          <div class="tab-content">
            <div role="tabpanel" class="tab-pane active" id="personal-info">
              <h3 class="header header_plain">Personal Info</h3>
              <div class="table-responsive">
                <table class="table">
                  <tbody>
                    <tr>
                      <th scope="row">Email</th>
                      <td>{{ user.email }}</td>
                    </tr>
                    <tr>
                      <th scope="row">Phone Number</th>
                      <td>{{ user.phone_number }}</td>
                    </tr>
                    <tr>
                      <th scope="row">Hours Completed</th>
                      <td>{{ hoursCompleted() }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <!-- / .table-responsive -->
            </div>
            <div role="tabpanel" class="tab-pane" id="my-events">
              <h3 class="header header_plain">My Events</h3>
              <div class="row">
                <div v-for="event in user.events">
                  <div class="col-xs-12 col-sm-6 col-md-4">
                    <div class="portfolio__item">
                      <div class="portfolio__caption">
                        <h3 class="portfolio__title" id="my_events">
                          <router-link :to="`/events/${event.id}`">
                            <h3>{{ event.title }}</h3>
                          </router-link>
                        </h3>
                        <div class="portfolio__intro">
                          <p>{{ event.tags }}</p>
                          <p>
                            Start Time:
                            {{ $parent.formatDate(event.event_start) }}
                          </p>
                          <p>End Time: {{ $parent.eventEnd(event) }}</p>
                        </div>
                        <p>{{ event.tags }}</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div role="tabpanel" class="tab-pane" id="attending-events">
              <h3 class="header header_plain">Attending Events</h3>
              <div class="row">
                <div
                  v-for="event in user.attending_events"
                  v-if="futureEvent(event.event_start) == true"
                >
                  <div class="col-xs-12 col-sm-6 col-md-4">
                    <div class="portfolio__item">
                      <div class="portfolio__caption">
                        <h3 class="portfolio__title">
                          <router-link :to="`/events/${event.id}`">
                            <h3>{{ event.title }}</h3>
                          </router-link>
                        </h3>
                        <div class="portfolio__intro">
                          <p>{{ event.tags }}</p>
                          <p>
                            Start Time:
                            {{ $parent.formatDate(event.event_start) }}
                          </p>
                          <p>End Time: {{ $parent.eventEnd(event) }}</p>
                        </div>
                        <p>{{ event.tags }}</p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <!-- / .row -->
            </div>
            <div role="tabpanel" class="tab-pane" id="past-events">
              <h3 class="header header_plain">Past Events</h3>
              <div class="row">
                <div class="col-xs-12 col-sm-6 col-md-4">
                  <div
                    v-for="event in user.attending_events"
                    v-if="futureEvent(event.event_start) == false"
                  >
                    <div class="portfolio__item">
                      <!-- Captions -->
                      <div class="portfolio__caption">
                        <h3 class="portfolio__title">
                          <router-link :to="`/events/${event.id}`">
                            <h3>{{ event.title }}</h3>
                          </router-link>
                        </h3>
                        <div class="portfolio__intro">
                          <p>{{ event.tags }}</p>
                          <p>
                            Start Time:
                            {{ $parent.formatDate(event.event_start) }}
                          </p>
                          <p>End Time: {{ $parent.eventEnd(event) }}</p>
                        </div>
                        <p>{{ event.tags }}</p>
                      </div>
                    </div>

                    <!-- <p>Future Event?: {{ futureEvent(event.event_start) }}</p> -->
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- / .tab-content -->
        </div>
      </div>
      <!-- / .row -->
    </div>
    <!-- / .container -->
  </div>
</template>

<style>
#icon {
  width: 2.5%;
  height: 2.5%;
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
    hoursCompleted: function() {
      var completedHours = 0;
      var today = moment().format();
      this.user.attending_events.forEach(function(event) {
        var eventStart = moment(event.event_start).format();
        if (today < eventStart === false) {
          completedHours += event.duration;
        }
      });
      // return completedHours;
      return completedHours;
    },
  },
};
</script>
