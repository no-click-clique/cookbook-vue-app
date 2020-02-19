<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>{{ recipes }}</p>
  </div>
</template>

<style>
</style>

<script>
export default {
  data: function() {
    return {
      message: "Hello World",
      recipes: ["Pizza", "Peanut Butter Cookies", "Empty Sandwich"]
    };
  },
  created: function() {},
  methods: {}
};
</script>