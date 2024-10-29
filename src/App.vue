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

    <!-- Movie List Display -->
    <div v-if="movies.length">
      <h2>Movies List</h2>
      <ul>
        <li v-for="(movie, index) in movies" :key="index">
          <h3>{{ movie.title }} ({{ movie.releaseDate }})</h3>
          <p><strong>Genre:</strong> {{ movie.genre }}</p>
          <p><strong>Rating:</strong> {{ movie.rating }}</p>
          <p>{{ movie.summary }}</p>
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
        summary: ''
      },
      movies: []
    };
  },
  methods: {
    addMovie() {
      // Push a copy of the newMovie object into movies array
      this.movies.push({ ...this.newMovie });

      // Clear the form fields
      this.newMovie = {
        title: '',
        genre: '',
        rating: null,
        releaseDate: '',
        summary: ''
      };
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

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 10px 0;
}
</style>
