<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      <h2>New Recipe</h2>
      Title: <input type="text" v-model="newRecipeTitle"><br>
      Ingredients: <input type="text" v-model="newRecipeIngredients"><br>
      Directions: <input type="text" v-model="newRecipeDirections"><br>
      Prep Time: <input type="number" v-model="newRecipePrepTime"><br>
      Image Url: <input type="text" v-model="newRecipeImageUrl"><br>
      <button v-on:click="createRecipe()">Create</button>
    </div>

    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <img v-bind:src="recipe.image_url" alt=""><br>
      <button v-on:click="recipe.showExtraInfo = !recipe.showExtraInfo">More Info</button>
      <div v-if="recipe.showExtraInfo">
        <p>Ingredients: {{ recipe.ingredients }}</p>
        <p>Directions: {{ recipe.directions }}</p>
        <p>Prep time: {{ recipe.formatted.prep_time }}</p>
        <div>
          {{ recipe }}
          <h4>Edit Recipe</h4>
          Title: <input type="text" v-model="recipe.title"><br>
          Ingredients: <input type="text" v-model="recipe.ingredients"><br>
          Directions: <input type="text" v-model="recipe.directions"><br>
          Prep Time: <input type="text" v-model="recipe.prep_time"><br>
          Image Url: <input type="text" v-model="recipe.image_url"><br>
          <button v-on:click="updateRecipe(recipe)">Update</button>
        </div>
      </div>
    </div>

  </div>
</template>

<style>
 img {
  width: 250px;
 }
</style>

<script>
// var axios = require("axios");
import axios from 'axios';
export default {
  data: function() {
    return {
      message: "Cookbook Frontend",
      recipes: [],
      newRecipeTitle: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      newRecipePrepTime: null,
      newRecipeImageUrl: "",
    };
  },
  // respone = HTTP.get("/api/recipes")
  // @recipes = response.parse
  created: function() {
    axios.get("/api/recipes").then(response => {
      response.data.forEach(recipe => {
        recipe.showExtraInfo = false;
      });
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {
    createRecipe: function() {
      var params = {
        title: this.newRecipeTitle,
        ingredients: this.newRecipeIngredients,
        directions: this.newRecipeDirections,
        prep_time: this.newRecipePrepTime,
        image_url: this.newRecipeImageUrl
      };
      axios.post("/api/recipes", params)
      .then(response => {
        console.log("Success!", response.data);
        this.recipes.push(response.data);
      })
      .catch(error => {
        console.log(error.response.data.errors);
      });
    },
    updateRecipe: function(recipe) {
      var params = {
        title: recipe.title,
        ingredients: recipe.ingredients,
        directions: recipe.directions,
        prep_time: recipe.prep_time,
        image_url: recipe.image_url
      };
      axios.patch(`/api/recipes/${recipe.id}`, params)
      .then(response => {
        console.log("Success!", response.data);
      })
      .catch(error => {
        console.log(error.response.data.errors);
      });
    }
  }
};
</script>