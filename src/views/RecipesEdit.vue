<template>
  <div class="recipes-edit">

    <form v-on:submit.prevent="submit()">
      <h1>Edit Recipe</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Title:</label> 
        <input type="text" class="form-control" v-model="recipe.title">
      </div>
      <div class="form-group">
        <label>Ingredients:</label>
        <input type="text" class="form-control" v-model="recipe.ingredients">
      </div>
      <div class="form-group">
        <label>Directions:</label>
        <input type="text" class="form-control" v-model="recipe.directions">
      </div>
      <div class="form-group">
        <label>Prep Time</label>
        <input type="number" class="form-control" v-model="recipe.prep_time">
      </div>
      <div class="form-group">
        <label>Image Url</label>
        <input type="text" class="form-control" v-model="recipe.image_url">
      </div>
      <input type="submit" class="btn btn-primary" value="Update Recipe">
    </form>
    
    <div>
      <button v-on:click="destroyRecipe()">Delete Recipe</button>
    </div>
    

  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {},
      errors: []
    };
  },
  created: function() {
    axios.get(`/api/recipes/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.recipe = response.data;
    });
  },
  methods: {
    submit: function() {
      var params = {
        title: this.recipe.title,
        ingredients: this.recipe.ingredients,
        directions: this.recipe.directions,
        prep_time: this.recipe.prep_time,
        image_url: this.recipe.image_url
      };
      axios
        .patch(`/api/recipes/${this.recipe.id}`, params)
        .then(response => {
          this.$router.push(`/recipes/${this.recipe.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyRecipe: function() {
      axios.delete(`/api/recipes/${this.recipe.id}`)
      .then(response => {
        console.log("Success", response.data);
        this.$router.push("/recipes");
      });
    }
  }
};
</script>