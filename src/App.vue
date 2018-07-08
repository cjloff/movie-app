<template>
  <div id="app">
    <div class="header">
      <h1>Movie Filter App</h1>
      <h2>Filter by Rating</h2>
      <input type="number" value="3" step="0.5" min="0" max="10" v-model="rating">
    </div>
    <div class="page">
      <h2>Top Movies</h2>
        <transition-group name="list" tag="div" class="movie-list">
          <app-movie-list
          v-for="movie in filteredMovies"
          v-bind:movie="movie"
          v-bind:key="movie.id"
          v-bind:genres="genreList"
          />
        </transition-group>
        <p v-if="filteredMovies.length == 0">Sorry no movies to see here! :(</p>
    </div>
  </div>
</template>

<script>

import MovieList from './components/MovieList'

const axios = require('axios')

const apiKey = 'de85f77f783771ffb98c2924e950206c'

export default {
  name: 'App',
  components: {
    'app-movie-list': MovieList
  },
  data () {
    return {
      movieList: [],
      genreList: [],
      rating: 3
    }
  },
  mounted () {
    axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=${apiKey}`)
      .then(response => { this.movieList = response.data.results })
    axios.get(`https://api.themoviedb.org/3/genre/movie/list?api_key=${apiKey}`)
      .then(response => { this.genreList = response.data.genres })
  },
  computed: {
    filteredMovies: function () {
      let movieByRating = this.movieList.filter(movie => movie.vote_average > this.rating)
      // order movies by popularity
      return movieByRating.sort((a, b) => { return b.popularity - a.popularity })
    }
  }
}
</script>

<style>

body {
    background-color: #081c24
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
}
* {
  box-sizing: border-box;
}
.header {
  padding: 20px;
  text-align: center;
  background: rgba(255,255,255,0.2);
}
p {
  text-align: center;
  font-weight: bold;
  font-size: 16px;
}
h2 {
  padding-bottom: 20px;
  margin-bottom: 20px;
  border-bottom: 1px solid rgba(255,255,255,0.4);
  text-align: center;
}
.page {
  padding: 20px;
}
.movie-list {
    display: flex;
    width: 100%;
    flex-wrap: wrap;
    padding-bottom: 20px;
}
.filter-list {
    display: flex;
    width: 100%;
    justify-content: space-between;
    flex-wrap: wrap;
    padding-bottom: 20px;
}
input[type=number] {
  padding: 5px;
  border: 0px;
  border: 1px solid rgba(255,255,255,0.5);
  background-color: rgba(255,255,255,0.4);
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>
