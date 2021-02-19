<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <h2>Add a new movie:</h2>
      Title:
      <input type="text" v-model="newTitle" />
      <br />
      Year:
      <input type="text" v-model="newYear" />
      <br />
      Plot:
      <input type="text" v-model="newPlot" />
      <div v-bind:class="{ 'text-danger': newPlot.length > 500 }">{{ 500 - newPlot.length }} characters remaining</div>
      <br />
      Director:
      <input type="text" v-model="newDirector" />
      <br />
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
