<template>
  <div id="app">
    <nav id="nav" class="navbar navbar-expand-lg navbar-light bg-light mb-5">
      <router-link class="navbar-brand" to="/">Movie App</router-link>

      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/">All Movies</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/movies/new">New Movie</router-link>
          </li>
          <li class="nav-item" v-if="isLoggedIn()">
            <router-link class="nav-link" to="/logout">Logout</router-link>
          </li>
          <li class="nav-item" v-if="!isLoggedIn()">
            <router-link class="nav-link" to="/signup">Signup</router-link>
          </li>
          <li class="nav-item" v-if="!isLoggedIn()">
            <router-link class="nav-link" to="/login">Login</router-link>
          </li>
        </ul>
      </div>
    </nav>
    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      return localStorage.getItem("jwt");
    },
  },
};
</script>
