<template>
  <div class="movies-index">
    <ul v-for="movie in movies" :key="movie.id">
      <li>
        <h2>{{ movie.title }}</h2>
        <p>Year: {{ movie.year }}</p>
        <p>Plot: {{ movie.plot }}</p>
        <p>Director: {{ movie.director }}</p>
        <router-link :to="`/movies/${movie.id}`">More info</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movies: [],
      newTitle: "",
      newYear: "",
      newPlot: "",
      newDirector: "",
    };
  },
  created: function() {
    axios
      .get("/api/movies")
      .then(response => {
        this.movies = response.data;
      })
      .catch(error => {
        console.log(error.response.data);
      });
  },
};
</script>
