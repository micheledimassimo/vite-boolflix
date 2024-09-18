<script>
import axios from 'axios';


export default {
  data() {
    return { 
      searchText: '',
      apiKey: '5478ac7f2d33ff40208455fe556ca0c3',
      movies: [],
      series: [],
      basicImgUrl: 'https://image.tmdb.org/t/p/',
      basicImgSize: 'w342'
    }
  },
  components: {
    
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
      //funzione per cambiare le lingue in bandiere
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
   
    <header class="d-flex justify-content-between p-4">
      <h1>
        Boolflix
      </h1>
      <div>
        <input @keyup.enter="search" class="form-control" v-model="searchText" type="text" placeholder="Cerca film o serie TV">
        
      </div>
    </header>
    
    <main>
      

      <div class="my-container">
        <h2>
          Movies
        </h2>
        <ol class="my-row">
          <li class="my-col-1-4"  v-for="(movie, i) in movies " :key="i">
            <ul >
              <li>
                <img :src="basicImgUrl + basicImgSize + movie.poster_path" :alt="movie.title">
              </li>
              <li>
                Titolo: {{ movie.title }}
              </li>
              <li>
                Titolo originale: {{ movie.original_title }}
              </li>
              <li>
                Lingua: <img class="flag-img" :src="getFlags(movie.original_language)" alt="">
              </li>
              <div>
                  Voto: {{ Math.ceil(movie.vote_average / 2) }}
              </div>
              <div>
                <i v-for="x in ( Math.ceil(movie.vote_average / 2)) " :key="x" class="fa-solid fa-star stars"></i>
                <i v-for="x in (5 - Math.ceil(movie.vote_average / 2)) " :key="x" class="fa-regular fa-star"></i>
              </div>

            </ul>
            <hr>
          </li>
        </ol>

        <hr>


        <h2>
          Series
        </h2>
        <ol class="my-row">
          <li class="my-col-1-4"  v-for="(movie, i) in movies " :key="i">
            <ul>
              <li>
                <img :src="basicImgUrl + basicImgSize + serie.poster_path" :alt="serie.title">
              </li>
              <li>
                Titolo: {{ serie.name }}
              </li>
              <li>
                Titolo originale: {{ serie.original_name }}
              </li>
              <li>
                Lingua: <img class="flag-img" :src="getFlags(serie.original_language)" alt="">
              </li>
              <li>
                <div>
                  Voto: {{ Math.ceil(serie.vote_average / 2) }}
                </div>
                <div>
                <i v-for="x in ( Math.ceil(serie.vote_average / 2)) " :key="x" class="fa-solid fa-star stars"></i>
                <i v-for="x in (5 - Math.ceil(serie.vote_average / 2)) " :key="x" class="fa-regular fa-star"></i>
              </div>
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
* {
  background-color: $mainBgColor;
}
h1 {
  color: red;
}
img {
  max-width: 100%;
  object-fit: cover;
  &.flag-img {
    max-width: 50px;
  }
}
.my-container {
  h2 {
    color: white;
  }
  ol {
    list-style: none;
    li > ul {
      list-style: none;
      li {
        font-size: 0.85rem;
        color: white;
        > div {
          &.stars {
            color: white;
          }
        }
      }
    }
  }
}

</style>
