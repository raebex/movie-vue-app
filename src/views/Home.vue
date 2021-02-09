<template>
  <div class="home">
    <h2>Add a new movie:</h2>
    Title: <input type="text" v-model="newTitle">
    <br>
    Year: <input type="text" v-model="newYear">
    <br>
    Plot: <input type="text" v-model="newPlot">
    <br>
    Director: <input type="text" v-model="newDirector">
    <br>
    <button v-on:click="addMovie">Add movie</button>
    
    <ul v-for="movie in movies" :key="movie.id">
      <li>
        <h2>{{ movie.title }}</h2>
        <p>Year: {{ movie.year }}</p>
        <p>Plot: {{ movie.plot }}</p>
        <p>Director: {{ movie.director }}</p>
      </li>
    </ul>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movies: [],
      newTitle: "",
      newYear: "",
      newPlot: "",
      newDirector: ""
    };
  },
  created: function() {
    this.listMovies();
  },
  methods: {
    listMovies: function() {
      axios
        .get("/api/movies")
        .then(response => {
          this.movies = response.data;
        })
        .catch(error => {
          console.log(error.response.data);
        });
    },
    addMovie: function() {
      var params = {
        title: this.newTitle,
        year: this.newYear,
        plot: this.newPlot,
        director: this.newDirector
      };

      axios
        .post("/api/movies", params, {
          headers: {
            Authorization: "Bearer [YOUR_JWT_TOKEN]"
          }
        })
        .then(response => {
          this.movies.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data);
        });
    }
  }
};
</script>