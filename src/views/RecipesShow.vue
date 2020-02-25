<template>
  <div class="recipes-show">

    <h2>Title: {{ recipe.title }}</h2>
    <img v-bind:src="recipe.image_url" alt=""><br>
    <h4>Ingredients: {{ recipe.ingredients }}</h4>
    <h4>Directions: {{ recipe.directions }}</h4>
    <h5>Prep Time: {{ recipe.formatted.prep_time }}</h5>

    <p>Created by: {{ recipe.chef }}</p>

    <router-link :to="`/recipes/${recipe.id}/edit`">Edit</router-link>

  </div>
</template>

<style>
</style>

<script>
import axios from 'axios';
export default {
  data: function() {
    return {
      recipe: {}
    };
  },
  created: function() {
    axios.get(`/api/recipes/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.recipe = response.data;
    });
  },
  methods: {}
};
</script>