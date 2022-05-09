<template>
  <Loading v-if="$fetchState.pending"></Loading>
  <div v-else class="container single-movie">
    <NuxtLink
      class="bg-blue-300 hover:bg-blue-600 rounded px-4 py-1 mt-3"
      :to="{ name: 'index' }"
    >
      Back
    </NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
        <img
          :src="`https://image.tmdb.org/t/p/w185${movie.poster_path}`"
          alt=""
        />
      </div>
      <div class="movie-content">
        <h1>Title: {{ movie.title }}</h1>
        <p class="movie-fact tagline">
          <span>Tagline:</span>
          "{{ movie.tagline }}"
        </p>
        <p class="movie-fact">
          <span>Released:</span>
          {{
            new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>Duration:</span>
          {{ movie.runtime }} minutes
        </p>
        <p class="movie-fact">
          <span>Revenue:</span>
          {{
            movie.revenue.toLocaleString('en-us', {
              style: 'currency',
              currency: 'USD',
            })
          }}
        </p>
        <p class="movie-fact">
          <span>OVerview:</span>
          {{ movie.overview }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'single-movies',
  head() {
    return {
      title: this.movie.title,
    }
  },
  data() {
    return {
      movie: null,
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
  fetchDelay: 1000,
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=d58d37fbedba880d3cef5cd792eb52d1&language=en-US&page=1`,
      )
      const result = await data
      this.movie = result.data
    },
  },
}
</script>

<style></style>
