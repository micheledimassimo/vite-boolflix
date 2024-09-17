<script>
import axios from 'axios';
import AppHeader from './components/AppHeader.vue';

export default {
  data() {
    return { 
      searchText: '',
      apiKey: '5478ac7f2d33ff40208455fe556ca0c3',
      movies: [],
      series: []
    }
  },
  components: {
    AppHeader
  },
  methods: {
    search() {
      axios  //chiamata api
      .get('https://api.themoviedb.org/3/search/movie', {
        params: {
          api_key: this.apiKey,
          query: this.searchText,
        }
      })
      .then((resp) => {
        console.log(resp.data);
        //salvo i dati restituiti in array
        this.movies = resp.data.results;
      });
    }
  }
}
</script>

<template>
  <div>
   
    <AppHeader />
    
    <main>
      <div>
        <input v-model="searchText" type="text" placeholder="Cerca film o serie TV">
        <button @click="search">
          Cerca
        </button>
      </div>

      <div>
        <ol>
          <li v-for="(movie, i) in movies " :key="i">
            <ul>
              <li>
                Titolo: {{ movie.title }}
              </li>
              <li>
                Titolo originale: {{ movie.original_title }}
              </li>
              <li>
                Lingua: {{ movie.original_language }}
              </li>
              <li>
                voto: {{ movie.vote_average }}
              </li>

            </ul>
            <hr>
          </li>
        </ol>
      </div>
    </main>
  </div>
</template>

<style lang="scss">
@use 'assets/scss/main' as *;
// Import all of Bootstrap's CSS
@import "bootstrap/scss/bootstrap";
</style>
