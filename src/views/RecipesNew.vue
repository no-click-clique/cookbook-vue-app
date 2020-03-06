<template>
  <div class="recipes-new">

    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>New Recipe</h1>
        <img v-if="status" v-bind:src="`https://http.cat/${status}`">
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Title:</label> 
          <input type="text" class="form-control" v-model="title">
        </div>
        <div class="form-group">
          <label>Ingredients:</label>
          <input type="text" class="form-control" v-model="ingredients">
        </div>
        <div class="form-group">
          <label>Directions:</label>
          <input type="text" class="form-control" v-model="directions">
        </div>
        <div class="form-group">
          <label>Prep Time:</label>
          <input type="number" class="form-control" v-model="prepTime">
        </div>
        <div class="form-group">
          <label>Image:</label>
          <input type="file" class="form-control" v-on:change="setFile($event)" ref="fileInput">
        </div>
        <input type="submit" class="btn btn-primary" value="Create Recipe">
      </form>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      title: "",
      ingredients: "",
      directions: "",
      prepTime: "",
      image: "",
      errors: [],
      status: ""
    };
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    submit: function() {
      var formData = new FormData();

      formData.append("title", this.title);
      formData.append("ingredients", this.ingredients);
      formData.append("directions", this.directions);
      formData.append("prep_time", this.prepTime);
      formData.append("image", this.image);

      axios
        .post("/api/recipes", formData)
        .then(response => {
          this.$router.push(`/recipes/${response.data.id}`);
        })
        .catch(error => {
          this.status = error.response.status;
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>