<template>
  <div class="movies-new">
    <h1>Add a new movie:</h1>
    <ul>
      <li class="text-danger" v-for="error in errors" v-bind:key="error">
        {{ error }}
      </li>
    </ul>
    <form v-on:submit.prevent="createMovie()" class="mb-5">
      <div class="form-group">
        <label>
          Title:
          <input class="form-control" type="text" v-model="newTitle" />
        </label>
      </div>
      <div class="form-group">
        <label>
          Year:
          <input class="form-control" type="text" v-model="newYear" />
        </label>
      </div>
      <div class="form-group">
        <label>
          Plot:
          <textarea class="form-control" v-model="newPlot" rows="10" cols="40"></textarea>
        </label>
        <div v-bind:class="{ 'text-danger': newPlot.length > 500 }">
          {{ 500 - newPlot.length }} characters remaining
        </div>
      </div>
      <div class="form-group">
        <label>
          Director:
          <input class="form-control" type="text" v-model="newDirector" />
        </label>
      </div>
      <input type="submit" class="btn btn-primary" value="Add Movie" />
    </form>
  </div>
</template>

<style scoped>
.text-danger {
  color: red;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      newTitle: "",
      newYear: "",
      newPlot: "",
      newDirector: "",
      errors: [],
    };
  },
  methods: {
    createMovie: function() {
      var params = {
        title: this.newTitle,
        year: this.newYear,
        plot: this.newPlot,
        director: this.newDirector,
      };

      axios
        .post("/api/movies", params)
        .then(response => {
          console.log(response.data);
          this.$router.push("/");
        })
        .catch(error => {
          console.log(error.response.data);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
