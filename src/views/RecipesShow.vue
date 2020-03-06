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

    <!-- Button trigger modal -->
    <button v-if="recipe.user.id == $parent.getUserId()" type="button" class="btn btn-warning" data-toggle="modal" data-target="#editRecipeModal">
      Edit
    </button>

    <!-- Modal -->
    <div class="modal fade" id="editRecipeModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-body">
            <form v-on:submit.prevent="submit()">
              <h3>Edit Recipe</h3>
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
            
          </div>
          <div class="modal-footer">
            <button class="btn btn-danger" data-dismiss="modal" v-on:click="destroyRecipe()">Delete Recipe</button>
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>

          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<style>
</style>

<script>
import axios from 'axios';
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
          $('#editRecipeModal').modal('hide');
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