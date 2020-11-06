<template>
  <div class="users-edit">
    <form v-on:submit.prevent="updateUser()">
      <h1>Edit User Information</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>First Name:</label>
        <input type="text" class="form-control" v-model="user.first_name" />
      </div>
      <div class="form-group">
        <label>Last Name:</label>
        <input type="text" class="form-control" v-model="user.last_name" />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="text" class="form-control" v-model="user.email" />
      </div>
      <div class="form-group">
        <label>Phone Number:</label>
        <input type="text" class="form-control" v-model="user.phone_number" />
      </div>
      <div class="form-group">
        <label>Image:</label>
        <input type="text" class="form-control" v-model="user.image" />
      </div>
      <!-- <div class="form-group">
        <label>Current Password:</label>
        <input type="text" class="form-control" v-model="user.password" />
      </div> -->
      <div class="form-group">
        <label>New Password:</label>
        <input type="text" class="form-control" v-model="user.password" />
      </div>
      <input type="submit" class="btn btn-primary" value="Update" />
    </form>
    <button class="btn btn-danger" v-on:click="destroyMovie()">Delete</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      user: {},
      errors: [],
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.user = response.data;
    });
  },
  methods: {
    updateUser: function() {
      var params = {
        first_name: this.user.first_name,
        last_name: this.user.last_name,
        image: this.user.image,
        email: this.user.email,
        phone_number: this.user.phone_number,
        password: this.user.password,
      };
      axios
        .patch(`/api/users/${this.user.id}`, params)
        .then((response) => {
          this.$router.push(`/users/${this.user.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyMovie: function() {
      if (confirm("Are you sure you want to delete this account?")) {
        axios.delete(`/api/users/${this.user.id}`).then((response) => {
          console.log("Account Successfully Deleted", response.data);
          this.$router.push("/users");
        });
      }
    },
  },
};
</script>
