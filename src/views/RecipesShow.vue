<template>
  <div class="recipes-show">

    <div class="text-center">
      <h2>{{ recipe.title }}</h2>
      <img v-bind:src="recipe.image_url" alt="">
    </div>
    <h4>Ingredients</h4>
    <p v-for="ingredient in recipe.formatted.ingredients">{{ ingredient }}</p>
    <h4>Directions</h4>
    <p>{{ recipe.directions }}</p>
    <h5>Prep Time: {{ recipe.formatted.prep_time }}</h5>

    <p>Created by: {{ recipe.user.name }}</p>

    recipes user id: {{ recipe.user.id }}<br>
    current user id: {{ $parent.getUserId() }}<br>

    <router-link class="btn btn-warning" v-if="recipe.user.id == $parent.getUserId()" :to="`/recipes/${recipe.id}/edit`">Edit</router-link>

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