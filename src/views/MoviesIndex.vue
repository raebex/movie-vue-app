<template>
  <div class="movies-index">
    <input type="text" placeholder="Search by title" v-model="titleFilter" list="titleList" />

    <datalist id="titleList">
      <option v-for="movie in movies" :key="movie.id">{{ movie.title }}</option>
    </datalist>

    <fieldset>
      <legend>Sort:</legend>
      <ul>
        <li>
          <label for="alphabetically">alphabetically</label>
          <input
            v-on:change="sort('title', 1)"
            type="radio"
            id="alphabetically"
            name="sort"
            value="alphabetically"
            v-model="picked"
          />
        </li>
        <li>
          <label for="oldest">oldest</label>
          <input v-on:change="sort('year', 1)" type="radio" id="oldest" name="sort" value="oldest" v-model="picked" />
        </li>
        <li>
          <label for="newest">newest</label>
          <input v-on:change="sort('year', -1)" type="radio" id="newest" name="sort" value="newest" v-model="picked" />
        </li>
      </ul>
    </fieldset>

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
      picked: "alphabetically",
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
    sort: function(field, direction) {
      this.sortField = field;
      this.sortOrder = direction;
    },
  },
};
</script>
