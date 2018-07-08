<template>
<div class="movie-item">
    <img v-bind:src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`">
    <h3>{{movie.title}}</h3>
    <ul>
        <li v-for="genre in genreListing" v-bind:key="genre">
            {{genre.name}}
        </li>
    </ul>
</div>
</template>

<script>
export default {
  props: ['movie', 'genres'],
  computed: {
    genreListing: function () {
      return this.genres.filter((o) => this.movie.genre_ids.includes(o.id))
    }
  }
}
</script>
<style>
    div.movie-item {
        width: 25%;
        padding: 30px;
    }
    ul {
        padding: 0px;
        margin: 0px;
    }
    li {
        list-style: none;
    }
    img {
        max-width: 100%
    }
</style>
