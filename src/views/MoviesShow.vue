<template>
  <div class="movies-show">
    <div>
      <h1>{{ movie.title }}</h1>
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <p>Director: {{ movie.director }}</p>
      <p v-if="movie.actors.length > 0">
        Actors:
        <span v-for="(actor, index) in movie.actors" :key="actor.id">
          {{ actor.first_name }} {{ actor.last_name }}
          <span v-if="index < movie.actors.length - 1">,</span>
        </span>
      </p>
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
