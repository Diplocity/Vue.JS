<template>
  <div id="app" class="container">
    <h1>Movie Collection</h1>

    <!-- Movie Form -->
    <form @submit.prevent="addMovie" class="mb-4">
      <div class="mb-3">
        <label for="title" class="form-label">Title:</label>
        <input type="text" v-model="newMovie.title" class="form-control" required />
      </div>
      <div class="mb-3">
        <label for="genre" class="form-label">Genre:</label>
        <input type="text" v-model="newMovie.genre" class="form-control" required />
      </div>
      <div class="mb-3">
        <label for="rating" class="form-label">Rating:</label>
        <input type="number" v-model="newMovie.rating" min="0" max="10" step="0.1" class="form-control" required />
      </div>
      <div class="mb-3">
        <label for="releaseDate" class="form-label">Release Date:</label>
        <input type="date" v-model="newMovie.releaseDate" class="form-control" required />
      </div>
      <div class="mb-3">
        <label for="summary" class="form-label">Summary:</label>
        <textarea v-model="newMovie.summary" class="form-control" required></textarea>
      </div>
      <button type="submit" class="btn btn-success">Add Movie</button>
    </form>

    <!-- Search Movies -->
    <div class="mb-3">
      <input type="text" v-model="searchQuery" @input="debouncedSearch" placeholder="Search movies..." class="form-control" />
    </div>

    <!-- Movie List Display -->
    <div v-if="filteredMovies.length">
      <h2>Movies List</h2>
      <ul class="list-group">
        <li
          v-for="(movie, index) in filteredMovies"
          :key="index"
          class="list-group-item d-flex justify-content-between align-items-start"
          @mouseover="hovered = index"
          @mouseleave="hovered = null"
          :class="{'bg-light': hovered === index}"
        >
          <div>
            <h3>{{ movie.title }} ({{ movie.releaseDate }})</h3>
            <p><strong>Genre:</strong> {{ movie.genre }}</p>
            <p><strong>Rating:</strong> {{ movie.rating }}</p>
            <p>{{ movie.summary }}</p>
          </div>
          <button @click="confirmDelete(index)" class="btn btn-danger">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css';
import debounce from 'lodash.debounce';

export default {
  data() {
    return {
      newMovie: {
        title: '',
        genre: '',
        rating: null,
        releaseDate: '',
        summary: ''
      },
      movies: [],
      searchQuery: '',
      filteredMovies: [],
      hovered: null
    };
  },
  methods: {
    addMovie() {
      this.movies.push({ ...this.newMovie });
      this.newMovie = {
        title: '',
        genre: '',
        rating: null,
        releaseDate: '',
        summary: ''
      };
      this.searchMovies();
    },
    searchMovies() {
      const query = this.searchQuery.toLowerCase();
      this.filteredMovies = this.movies.filter(
        movie =>
          movie.title.toLowerCase().includes(query) ||
          movie.genre.toLowerCase().includes(query) ||
          movie.summary.toLowerCase().includes(query)
      );
    },
    confirmDelete(index) {
      if (confirm('Are you sure you want to delete this movie?')) {
        this.movies.splice(index, 1);
        this.searchMovies();
      }
    },
    debouncedSearch: debounce(function () {
      this.searchMovies();
    }, 300)
  },
  created() {
    this.filteredMovies = this.movies;
  }
};
</script>

<style scoped>
@import 'bootstrap/dist/css/bootstrap.css';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  max-width: 800px;
  margin: 0 auto;
}

.list-group-item:hover {
  background-color: #f8f9fa;
}

button {
  padding: 10px;
}

button:hover {
  opacity: 0.9;
}
</style>
