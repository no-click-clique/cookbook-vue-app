<template>
  <div class="recipes-index text-center">

    <div v-for="recipe in recipes" v-on:click="currentRecipe = recipe" v-bind:class="{ selected: recipe === currentRecipe }">
      <h2>Title: {{ recipe.title }}</h2>
      <router-link :to="`/recipes/${recipe.id}`">
        <img v-bind:src="recipe.image_url" alt=""><br>
      </router-link>
      <h5>Prep time: {{ recipe.prep_time }}</h5>
    </div>

  </div>
</template>

<style>
.selected {
  color: white;
  background-color: steelBlue;
  transition: background-color 1s ease;
}
</style>

<script>
import axios from 'axios';
export default {
  data: function() {
    return {
      recipes: [],
      currentRecipe: {}
    };
  },
  created: function() {
    axios.get("/api/recipes").then(response => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {}
};
</script>