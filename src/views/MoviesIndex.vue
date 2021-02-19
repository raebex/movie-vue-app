<template>
  <div class="movies-index">
    Search by title: <input type="text" v-model="titleFilter" list="titleList" />

    <datalist id="titleList">
      <option v-for="movie in movies" :key="movie.id">{{ movie.title }}</option>
    </datalist>

    <div>
      <button v-on:click="sort('title', 1, 1)" :class="{ active: activeButton === 1 }">Sort Alphabetically</button>
      <button v-on:click="sort('year', 1, 2)" :class="{ active: activeButton === 2 }">Sort by oldest</button>
      <button v-on:click="sort('year', -1, 3)" :class="{ active: activeButton === 3 }">Sort by newest</button>
    </div>

    <ul is="transition-group" appear enter-active-class="animated slideInUp" leave-active-class="animated slideOutDown">
      <li v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), sortField, sortOrder)" :key="movie.id">
        <h2>{{ movie.title }}</h2>
        <p>Year: {{ movie.year }}</p>
        <p>Plot: {{ movie.plot }}</p>
        <p>Director: {{ movie.director }}</p>
        <router-link :to="`/movies/${movie.id}`">More info</router-link>
      </li>
    </ul>
  </div>
</template>

<style scoped>
button.active {
  background: #000;
  color: #fff;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      movies: [],
      newTitle: "",
      newYear: "",
      newPlot: "",
      newDirector: "",
      titleFilter: "",
      sortField: "title",
      sortOrder: 1,
      activeButton: 1,
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
  methods: {
    sort: function(field, direction, button) {
      this.sortField = field;
      this.sortOrder = direction;
      this.activeButton = button;
    },
  }
};
</script>
