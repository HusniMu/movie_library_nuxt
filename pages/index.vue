<template>
  <div class="home">
    <!-- hero -->
    <Hero></Hero>

    <!-- search -->
    <div class="container search">
      <input
        @keyup.enter="$fetch"
        type="text"
        placeholder="Search"
        v-model.lazy="searchInput"
      />
      <button @click="clearSearch" v-show="searchInput !== ''" class="button">
        Clear Search
      </button>
    </div>

    <!-- movies -->
    <!-- searched movies -->
    <div class="container">
      <div v-if="searchInput !== ''" class="grid grid-cols-3 gap-4 movies">
        <div
          class="movie"
          v-for="(movie, index) in searchedMovies"
          :key="index"
        >
          <div class="movie-img">
            <img
              :src="`https://image.tmdb.org/t/p/w185${movie.poster_path}`"
              alt=""
            />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">{{ movie.overview }}</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink
              class="bg-blue-300 hover:bg-blue-600 rounded px-4 py-1 mt-3"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>

    <!-- loading -->
    <Loading v-if="$fetchState.pending"></Loading>

    <!-- default -->
    <div v-else class="container">
      <div v-if="searchInput === ''" class="grid grid-cols-3 gap-4 movies">
        <div class="movie" v-for="(movie, index) in movies" :key="index">
          <div class="movie-img">
            <img
              :src="`https://image.tmdb.org/t/p/w185${movie.poster_path}`"
              alt=""
            />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">{{ movie.overview }}</p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25) }}
              <span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink
              class="bg-blue-300 hover:bg-blue-600 rounded px-4 py-1 mt-3"
              :to="{ name: 'movies-movieid', params: { movieid: movie.id } }"
            >
              Get More Info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'IndexPage',
  head() {
    return {
      title: 'Movie App - Latest Streaming Movies',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content:
            'Lorem, ipsum dolor sit amet consectetur adipisicing elit. Iste, soluta.',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, streaming',
        },
      ],
    }
  },
  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: '',
    }
  },
  async fetch() {
    if (this.searchInput === '') {
      await this.getMovies()
      return
    }
    if (this.searchInput !== '') {
      await this.searchMovies()
    }
  },
  fetchDelay: 1000,
  methods: {
    async getMovies() {
      const data = axios.get(
        'https://api.themoviedb.org/3/movie/now_playing?api_key=d58d37fbedba880d3cef5cd792eb52d1&language=en-US&page=1',
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.movies.push(movie)
      })
      // console.log(this.movies)
      // console.log(result.data)
    },
    async searchMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=d58d37fbedba880d3cef5cd792eb52d1&language=en-US&page=1&query=${this.searchInput}`,
      )
      const result = await data
      this.searchedMovies = []
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie)
      })
    },
    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    },
  },
}
</script>
<style>
.loading {
  padding-top: 120px;
  align-items: flex-start;
}
</style>
