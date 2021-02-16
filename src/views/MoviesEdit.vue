<template>
  <div class="movies-edit">
    <form v-on:submit.prevent="updateMovie()">
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <h2>Edit movie:</h2>
      Title:
      <input type="text" v-model="movie.title" />
      <br />
      Year:
      <input type="text" v-model="movie.year" />
      <br />
      Plot:
      <input type="text" v-model="movie.plot" />
      <br />
      Director:
      <input type="text" v-model="movie.director" />
      <br />
      <input type="submit" class="btn btn-primary" value="Update" />
    </form>
    <button v-on:click="destroyMovie()">Destroy</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movie: {},
      errors: [],
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
  methods: {
    updateMovie: function() {
      var params = {
        title: this.movie.title,
        year: this.movie.year,
        plot: this.movie.plot,
        director: this.movie.director,
      };

      axios
        .patch(`/api/movies/${this.movie.id}`, params)
        .then(response => {
          console.log(response.data);
          this.$router.push(`/movies/${this.movie.id}`);
        })
        .catch(error => {
          console.log(error.response.data);
          this.errors = error.response.data.errors;
        });
    },
    destroyMovie: function() {
      if (confirm("Are you sure you want to delete this movie?")) {
        axios.delete(`/api/movies/${this.movie.id}`).then(response => {
          console.log(response.data);
          this.$router.push("/movies");
        });
      }
    },
  },
};
</script>
