<template>
  <div class="movies-show">
    <div>
      <h2>{{ movie.title }}</h2>
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <p>Director: {{ movie.director }}</p>
      <router-link :to="`/movies/${movie.id}/edit`">Edit movie</router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movie: {},
    };
  },
  created: function() {
    axios
      .get(`/api/movies/${this.$route.params.id}`)
      .then(response => {
        this.movie = response.data;
      })
      .catch(error => {
        console.log(error.response.data);
      });
  },
};
</script>
