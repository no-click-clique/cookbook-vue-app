<template>
  <div class="recipes-index">


    <div id="fh5co-work-section">
      <div class="container">
        <div class="row">
          <div class="col-md-6 col-md-offset-3 text-center fh5co-heading">
            <h2>All Recipes</h2>
            <p>What do you want to cook today?</p>
          </div>
        </div>
        <div>
          Search by title: <input type="text" v-model="titleFilter" list="titles">
          <button class="btn" v-on:click="sortAttribute = 'title'" v-bind:class="{'btn-primary': sortAttribute == 'title'}">Search by title</button>
          <button class="btn" v-on:click="sortAttribute = 'prep_time'" v-bind:class="{'btn-primary': sortAttribute == 'prep_time'}">Search by prep time</button>
        </div>
        <datalist id="titles">
          <option v-for="recipe in recipes">{{ recipe.title }}</option>
        </datalist>
        <transition-group class="row" appear enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
          <div v-for="recipe in orderBy(filterBy(recipes, titleFilter, 'title'), sortAttribute)" v-on:click="currentRecipe = recipe" v-bind:class="{ selected: recipe === currentRecipe }" class="col-md-4" v-bind:key="recipe.id">
            <router-link :to="`/recipes/${recipe.id}`" class="item-grid text-center">
              <div class="image" :style="`background-image: url(${recipe.image_url})`"></div>
              <div class="v-align">
                <div class="v-align-middle">
                  <h3 class="title">{{ recipe.title }}</h3>
                  <h5 class="category">Prep time: {{ recipe.prep_time }}</h5>
                  <h5 class="category">Created {{ relativeDate(recipe.created_at) }}</h5>
                </div>
              </div>
            </router-link>
          </div>
        </transition-group>
      </div>
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
import moment from 'moment';
import Vue2Filters from 'vue2-filters';

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      recipes: [],
      currentRecipe: {},
      titleFilter: "",
      sortAttribute: "title"
    };
  },
  created: function() {
    axios.get("/api/recipes").then(response => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {
    relativeDate: function(date) {
      return moment(date).fromNow();
    }
  }
};
</script>