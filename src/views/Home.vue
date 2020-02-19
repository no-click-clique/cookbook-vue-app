<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      <h2>New Recipe</h2>
      <button v-on:click="createRecipe()">Create</button>
    </div>


    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <img v-bind:src="recipe.image_url" alt="">
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
      <p>Prep time: {{ recipe.formatted.prep_time }}</p>
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
      recipes: []
    };
  },
  // respone = HTTP.get("/api/recipes")
  // @recipes = response.parse
  created: function() {
    axios.get("/api/recipes").then(response => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {
    createRecipe: function() {
      var params = {
        title: "example title",
        ingredients: "example ingredients",
        directions: "example directions",
        prep_time: "example prep time",
        image_url: "example image"
      };
      axios.post("/api/recipes", params)
      .then(response => {
        console.log("Success!", response.data);
        this.recipes.push(response.data);
      })
      .catch(error => {
        console.log(error.response.data.errors);
      });
    }
  }
};
</script>