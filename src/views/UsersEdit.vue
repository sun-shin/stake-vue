<template>
  <div class="users-edit">
    <ul>
      <li class="text-danger" v-for="error in errors">{{ error }}</li>
    </ul>
    <form v-on:submit.prevent="updateUser()">
      <h1>Edit User Information</h1>
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
        <input
          type="file"
          class="form-control"
          v-on:change="setFile($event)"
          ref="fileInput"
        />
      </div>
      <!-- <div class="form-group">
        <label>New Password:</label>
        <input type="text" class="form-control" v-model="user.password" />
      </div> -->
      <input type="submit" class="btn btn-primary" value="Update" />
    </form>
    <button class="btn btn-danger" v-on:click="destroyUser()">Delete</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      user: {},
      image: "",
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
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    updateUser: function() {
      var formData = new FormData();
      formData.append("first_name", this.user.first_name);
      formData.append("last_name", this.user.last_name);
      if (this.image) {
        formData.append("image", this.image);
      }
      formData.append("email", this.user.email);
      formData.append("phone_number", this.user.phone_number);
      formData.append("password", this.user.password);
      axios
        .patch(`/api/users/${this.user.id}`, formData)
        .then((response) => {
          this.$router.push(`/users/${this.user.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyUser: function() {
      if (confirm("Are you sure you want to delete this account?")) {
        axios.delete(`/api/users/${this.user.id}`).then((response) => {
          console.log("Account Successfully Deleted", response.data);
          delete axios.defaults.headers.common["Authorization"];
          localStorage.removeItem("jwt");
          localStorage.removeItem("user_id");
          this.$router.push("/login");
        });
      }
    },
  },
};
</script>
