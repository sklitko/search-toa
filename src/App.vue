<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12">
        <input type="text" placeholder="what are you looking for?" v-model="query" @input="autoComplete" class="form-control">
        <div class="panel-footer" v-if="results.length">
          <ul class="list-group">
            <li class="list-group-item" v-for="result in results" :key="result.id">
              {{ result.title }}
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-12 col-sm-6 col-md-6 col-lg-4 col-xl-2" v-for="film of films" v-bind:key="film.id">
        <div class="card">
          <img class="card-img-top" :src="'https://image.tmdb.org/t/p/w185_and_h278_bestv2/'+film.poster_path" alt="Card image cap">
          <div class="card-body">
            <h4 class="card-title">{{film.title}}</h4>
            <p class="card-text">{{film.overview}}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      films: [],
      query: '',
      results: []
    }
  },
  created () {
    axios.get('https://api.themoviedb.org/3/movie/popular', {
      params: {
        api_key: '6829616a40601676984639ee2f6ae37c',
        language: 'en-US',
        page: '1'
      }
    })
      .then(response => (this.films = response.data.results))
      .catch(error => console.log(error))
  },
  methods: {
    autoComplete () {
      this.films = []
      this.results = []
      if (this.query.length > 2) {
        axios.get('https://api.themoviedb.org/3/search/movie', {
          params: {
            api_key: '6829616a40601676984639ee2f6ae37c',
            language: 'en-US',
            page: '1',
            query: this.query,
            include_adult: false
          }
        }).then(response => { this.results = this.films = response.data.results })
      }
    }
  }
}
</script>

<style>

</style>
