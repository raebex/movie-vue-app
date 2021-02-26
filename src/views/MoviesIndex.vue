<template>
  <div class="movies-index">
    <div class="jumbotron">
      <h1 class="display-4">Movie App</h1>
      <p class="lead">
        A collection of my favorite movies.
      </p>
      <router-link class="btn btn-primary btn-lg" to="/movies/new" role="button">Add a movie</router-link>
    </div>

    <div class="d-flex justify-content-between mb-5">
      <div class="input-group">
        <div class="form-outline">
          <label class="form-label" for="form1">Search</label>
          <input
            type="search"
            id="form1"
            class="form-control"
            placeholder="Search by title"
            list="titleList"
            v-model="titleFilter"
          />
        </div>
      </div>

      <datalist id="titleList">
        <option v-for="movie in movies" :key="movie.id">{{ movie.title }}</option>
      </datalist>

      <fieldset class="input-group">
        <ul>
          <legend class="form-label">
            <small class="text-muted">Sort:</small>
          </legend>
          <li class="form-check form-check-inline">
            <input
              class="form-check-input"
              v-on:change="sort('title', 1)"
              type="radio"
              id="alphabetically"
              name="sort"
              value="alphabetically"
              v-model="picked"
            />
            <label class="form-check-label" for="alphabetically">alphabetically</label>
          </li>
          <li class="form-check form-check-inline">
            <input
              class="form-check-input"
              v-on:change="sort('year', 1)"
              type="radio"
              id="oldest"
              name="sort"
              value="oldest"
              v-model="picked"
            />
            <label class="form-check-label" for="oldest">oldest</label>
          </li>
          <li class="form-check form-check-inline">
            <input
              class="form-check-input"
              v-on:change="sort('year', -1)"
              type="radio"
              id="newest"
              name="sort"
              value="newest"
              v-model="picked"
            />
            <label class="form-check-label" for="newest">newest</label>
          </li>
        </ul>
      </fieldset>
    </div>
    <div
      class="row row-cols-1 row-cols-md-3"
      is="transition-group"
      appear
      enter-active-class="animated slideInUp"
      leave-active-class="animated slideOutDown"
    >
      <div
        class="col mb-4"
        v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), sortField, sortOrder)"
        :key="movie.id"
      >
        <div class="card h-100">
          <div class="card-body">
            <h2>{{ movie.title }}</h2>
            <p>Year: {{ movie.year }}</p>
            <p>Plot: {{ movie.plot }}</p>
            <p>Director: {{ movie.director }}</p>
            <router-link :to="`/movies/${movie.id}`">More info</router-link>
          </div>
        </div>
      </div>
    </div>
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
