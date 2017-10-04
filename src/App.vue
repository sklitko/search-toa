<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <input type="text" placeholder="SEARCH" v-model="query" @input="autoComplete" class="form-control">
        <ul class="nav nav-tabs">
          <li class="active">
            <a href="#home" data-toggle="tab">Главная
              <span>{{results.length}}</span>
            </a>
          </li>
          <li>
            <a href="#profile" data-toggle="tab">Профиль
              <span>{{results1.length}}</span>
            </a>
          </li>
          <li>
            <a href="#messages" data-toggle="tab">Сообщения
              <span>{{results2.length}}</span>
            </a>
          </li>
        </ul>

        <!-- Tab panes -->
        <div class="tab-content">
          <div class="tab-pane active" id="home">
            <div class="panel-footer" v-if="results.length">
              <ul class="list-group">
                <li class="list-group-item" v-for="result in results" :key="result.id">
                  <a :href="'http://www.themoviedb.org/movie/'+result.id">{{ result.title }}</a>
                </li>
              </ul>
            </div>
          </div>
          <div class="tab-pane" id="profile">
            <div class="panel-footer" v-if="results1.length">
              <ul class="list-group">
                <li class="list-group-item" v-for="result in results1" :key="result.id">
                  <a :href="'http://www.themoviedb.org/movie/'+result.id">{{ result.name }}</a>
                </li>
              </ul>
            </div>
          </div>
          <div class="tab-pane" id="messages">
            <div class="panel-footer" v-if="results2.length">
              <ul class="list-group">
                <li class="list-group-item" v-for="result in results2" :key="result.id">
                  <a :href="'http://www.themoviedb.org/movie/'+result.id">{{ result.name }}</a>
                </li>
              </ul>
            </div>
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
      query: '',
      results: [],
      results1: [],
      results2: []
    }
  },
  methods: {
    autoComplete () {
      this.results = []
      this.results1 = []
      this.results2 = []
      if (this.query.length > 2) {
        axios.get('https://api.themoviedb.org/3/search/movie', {
          params: {
            api_key: '6829616a40601676984639ee2f6ae37c',
            language: 'en-US',
            page: '1',
            query: this.query,
            include_adult: false
          }
        }).then(response => { this.results = response.data.results })

        axios.get('https://api.themoviedb.org/3/search/collection', {
          params: {
            api_key: '6829616a40601676984639ee2f6ae37c',
            language: 'en-US',
            page: '1',
            query: this.query
          }
        }).then(response => { this.results1 = response.data.results })

        axios.get('https://api.themoviedb.org/3/search/tv', {
          params: {
            api_key: '6829616a40601676984639ee2f6ae37c',
            language: 'en-US',
            page: '1',
            query: this.query
          }
        }).then(response => { this.results2 = response.data.results })
      }
    }
  }
}
</script>

<style>

</style>
