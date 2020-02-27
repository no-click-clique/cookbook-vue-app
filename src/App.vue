<template>
  <div id="app">

    
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="#">Cookbook</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <ul class="navbar-nav">
          <li class="nav-item active">
            <router-link class="nav-link" to="/">Home <span class="sr-only">(current)</span></router-link>
          </li>
          <li v-if="!isLoggedIn()" class="nav-item">
            <router-link class="nav-link" to="/signup">Signup</router-link>
          </li>
          <li v-if="!isLoggedIn()" class="nav-item">
            <router-link class="nav-link" to="/login">Login</router-link>
          </li>
          <li v-if="isLoggedIn()" class="nav-item">
            <router-link class="nav-link" to="/logout">Logout</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/recipes">All Recipes</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/recipes/new">New Recipe</router-link>
          </li>
        </ul>
      </div>
    </nav>

    <div class="container">
      <!-- every individual view in the views file gets interpolated here -->
      <router-view/>
    </div>

  </div>
</template>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      if (localStorage.getItem("jwt")) {
        return true;
      } else {
        return false;
      }
    },
    getUserId: function() {
      return localStorage.getItem("user_id");
    }
  }
};
</script>
