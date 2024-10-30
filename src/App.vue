<template>
  <div id="app">
    <h1>Movie Collection</h1>

    <!-- Movie Form -->
    <form @submit.prevent="addMovie">
      <div>
        <label for="title">Title:</label>
        <input type="text" v-model="newMovie.title" required />
      </div>
      <div>
        <label for="genre">Genre:</label>
        <input type="text" v-model="newMovie.genre" required />
      </div>
      <div>
        <label for="rating">Rating:</label>
        <input type="number" v-model="newMovie.rating" min="0" max="10" step="0.1" required />
      </div>
      <div>
        <label for="releaseDate">Release Date:</label>
        <input type="date" v-model="newMovie.releaseDate" required />
      </div>
      <div>
        <label for="summary">Summary:</label>
        <textarea v-model="newMovie.summary" required></textarea>
      </div>
      <button type="submit">Add Movie</button>
    </form>

    <!-- Search Movies -->
    <div class="search-bar">
      <input type="text" v-model="searchQuery" placeholder="Search by title or genre" />
    </div>

    <!-- Movie List Display -->
    <div v-if="filteredMovies.length">
      <h2>Movies List</h2>
      <ul>
        <li v-for="(movie, index) in filteredMovies" :key="movie.id">
          <h3>{{ movie.title }} ({{ formatDate(movie.releaseDate) }})</h3>
          <p><strong>Genre:</strong> {{ movie.genre }}</p>
          <p :style="{ color: getRatingColor(movie.rating) }"><strong>Rating:</strong> {{ movie.rating }}</p>
          <p>{{ movie.summary }}</p>
          <button @click="deleteMovie(index)">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newMovie: {
        title: '',
        genre: '',
        rating: null,
        releaseDate: '',
        summary: '',
        id: null
      },
      movies: [],
      searchQuery: ''
    };
  },
  computed: {
    filteredMovies() {
      // Filter movies based on the search query
      return this.movies.filter(movie =>
        movie.title.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
        movie.genre.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    }
  },
  methods: {
    addMovie() {
      // Assign a unique ID to each new movie
      this.newMovie.id = Date.now();
      this.movies.push({ ...this.newMovie });

      // Clear the form fields
      this.newMovie = {
        title: '',
        genre: '',
        rating: null,
        releaseDate: '',
        summary: '',
        id: null
      };
    },
    deleteMovie(index) {
      // Remove a movie from the list
      this.movies.splice(index, 1);
    },
    formatDate(date) {
      // Convert date to a more readable format
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(date).toLocaleDateString(undefined, options);
    },
    getRatingColor(rating) {
      // Change color based on rating
      if (rating >= 7) return 'green';
      else if (rating >= 4) return 'orange';
      else return 'red';
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
}

form {
  margin: 20px auto;
  max-width: 400px;
  display: flex;
  flex-direction: column;
}

form div {
  margin-bottom: 10px;
}

button {
  padding: 10px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #369d72;
}

.search-bar {
  margin: 20px auto;
  max-width: 400px;
}

.search-bar input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 10px 0;
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 4px;
}
</style>