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
      axios  //chiamata api per film
      .get('https://api.themoviedb.org/3/search/movie', {
        params: {
          api_key: this.apiKey,
          query: this.searchText,
        }
      })
      .then((resp) => {
        console.log(resp.data);
        //salvo i dati restituiti in array movies
        this.movies = resp.data.results;
      });
      axios  //chiamata api per serie tv
      .get('https://api.themoviedb.org/3/search/tv', {
        params: {
          api_key: this.apiKey,
          query: this.searchText,
        }
      })
      .then((resp) => {
        console.log(resp.data);
        //salvo i dati restituiti in array seires
        this.series = resp.data.results;
      });
    },
    getFlags(lang) {
      const languages = [
        'en',
        'it',
        'ja',
        'sp'
      ];
      if (languages.includes(lang)) {
        if (lang == 'en') {
          return '/flags/uk-flag.gif';
        }
        else if (lang == 'it') {
          return '/flags/it-flag.gif';
        }
        else if (lang == 'sp') {
          return '/flags/sp-flag.gif';
        }
        else if (lang == 'ja') {
          return '/flags/jp-flag.gif';
        }
      }
      else {
        return '/flags/Unknown_flag_-_European_version.png';
      }

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
        <h2>
          Movies
        </h2>
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
                Lingua: <img :src="getFlags(movie.original_language)" alt="">
              </li>
              <li>
                voto: {{ movie.vote_average }}
              </li>

            </ul>
            <hr>
          </li>
        </ol>

        <hr>


        <h2>
          Series
        </h2>
        <ol>
          <li v-for="(serie, i) in series " :key="i">
            <ul>
              <li>
                Titolo: {{ serie.name }}
              </li>
              <li>
                Titolo originale: {{ serie.original_name }}
              </li>
              <li>
                Lingua: <img :src="getFlags(serie.original_language)" alt="">
              </li>
              <li>
                voto: {{ serie.vote_average }}
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

img {
  max-width: 50px;
}
</style>
