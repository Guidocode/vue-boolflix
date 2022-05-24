<template>
  <div class="global-content d-flex flex-column">

    <!-- CONTENT TOP -->
    <div class="content-top d-flex justify-content-between align-items-center flex-wrap px-3">

      <div class="logo">B o o l f l i x</div>

      <div class="searchbars d-flex flex-wrap flex-md-nowrap">
        <SeachbarComp 
        @search="contentSearched"
        />
      </div>

    </div>
    <!-- /CONTENT TOP -->



    <!-- CONTENT BOTTOM -->
    <div class="content-bottom h-100">

      <!-- Se non c'è nessun errore vado avanti -->
      <div v-if="!isError" class="h-100">

        <!-- A fine caricamento stampo il contenuto se viene trovato -->
        <div v-if="!isLoading" class="gb-container h-100 py-5">

          <!-- Messaggio iniziale 'fai la richerca' -->
          <h2 :class="isBlock ? 'd-block' : 'd-none'" class="text-center py-5">{{initMessage}}</h2>

          <!-- Titolo dei contenuti più popolari -->
          <h4 :class="isBlock ? 'd-inline-block' : 'd-none'" class="title-content">I più visti su Netflix</h4>

          <!-- <div class="container-fluid"> -->
            <!-- MOVIES -->
            <div :class="isBlock ? 'd-none' : 'd-inline-block'" class="title-content mb-1">Movies</div>
            <!-- Messaggio stampato se la ricerca non produce risultati -->
            <h2 :class="isMoviesNotFound ? 'd-block' : 'd-none'" class="text-center py-5">{{moviesNotFound}}</h2>
            <div class="movies mb-5">
              <!-- row row-cols-1 row-cols-sm-2 row-cols-md-2 row-cols-lg-3 row-cols-xl-4 row-cols-xxl-5 -->

              <CardComp 
              v-for="movieItem in arrayMovies" :key="movieItem.id"
              :item="movieItem"
              />

            </div>
            <!-- /MOVIES -->

            <!-- TV SERIES -->
              <div :class="isBlock ? 'd-none' : 'd-inline-block'" class="title-content mb-1">Tv Series</div>
              <!-- Messaggio stampato se la ricerca non produce risultati -->
              <h2 :class="isSeriesNotFound ? 'd-block' : 'd-none'" class="text-center py-5">{{seriesNotFound}}</h2>
              <div class="tv-series">
                  <!-- row row-cols-1 row-cols-sm-2 row-cols-md-2 row-cols-lg-3 row-cols-xl-4 row-cols-xxl-5 -->
                <CardComp 
                v-for="serieItem in arraySeries" :key="serieItem.id"
                :item="serieItem"
                />
  
              </div>
            <!-- /TV SERIES -->

          <!-- </div> -->
        </div>
      
        <!-- Caricamento mentre avviene la ricerca -->
        <LoadingComp v-else />
      </div>

      <!-- Messaggio stampato in caso di errore API -->
      <div v-else class="text-center py-5">{{errorMessage}}</div>
    </div>
    <!-- /CONTENT BOTTOM -->

  </div>
</template>

<script>
import SeachbarComp from './SeachbarComp.vue';
import axios from 'axios';
import CardComp from './CardComp.vue';
import LoadingComp from './LoadingComp.vue';
export default {
  name: "GlobalContentComp",
  components: {
    SeachbarComp,
    CardComp,
    LoadingComp,
  },

  mounted(){
    this.getApi()
  },

  data(){
    return{
      apiUrl: 'https://api.themoviedb.org/3/search/',
      apiParams: {
        api_key: '331c13478db6d90df46ce36b05f18e36',
        language: 'it-IT',
        query: '',
      },
      arrayMovies: [],
      arraySeries: [],

      errorMessage: '',
      isError: false,

      isLoading: null,

      initMessage: 'SCEGLI COSA GUARDARE! FAI LA TUA RICERCA!',
      isBlock: true,

      moviesNotFound: 'NON CI SONO FILM CON QUESTO NOME...',
      isMoviesNotFound: false,

      seriesNotFound: 'NON CI SONO SERIE TV CON QUESTO NOME...',
      isSeriesNotFound: false
    }
  },

  methods:{
    getApi(type = ''){
      let defaultPopular = 'https://api.themoviedb.org/3/movie/popular';
      if(type !== ''){
        defaultPopular = this.apiUrl + type;
      }else{
        type = 'movie';
      }
      axios.get(defaultPopular, {
        params: this.apiParams
      })
      .then(resp => {
        console.log(resp.data);
      

        if(type == 'movie'){
          this.arrayMovies = resp.data.results;
          if(this.arrayMovies.length < 1) this.isMoviesNotFound = true;
        }else if(type == 'tv'){
          this.arraySeries = resp.data.results;
          if(this.arraySeries.length < 1) this.isSeriesNotFound = true;
        }

        this.isLoading = false;
      })
      .catch(error => {
        this.errorMessage = error;
        this.isError = true;
        console.log(this.errorMessage);
      })
    },

    contentSearched(inputRicerca){

      this.apiParams.query = inputRicerca.toLowerCase();

      this.isMoviesNotFound = false;
      this.getApi('movie');

      this.isSeriesNotFound = false;
      this.getApi('tv');

      this.isBlock = false;
      this.isLoading = true;
      console.log( 'sto cercando il contenuto', this.apiParams.query );  
    },
  },

  

}
</script>

<style lang="scss" scoped>
.global-content{
  height: 100vh;
  color: white;
  font-family: "Roboto";
  

  .content-top{
    min-height: 60px;
    background-image: linear-gradient(#1a2727, #2b3f3f);
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 999;

    .logo{
      color: #DB202C;
      font-size: 2rem;
      font-weight: 600;
      text-transform: uppercase;
    }
  }

  .content-bottom{
    background-color: #212323;
    flex-grow: 1;
    margin-top: 60px;
    overflow-y: scroll;

    .title-content{
    display: inline-block;
    text-transform: uppercase;
    color: white;
    background-color: rgba(61, 61, 246, 0.525);
    padding: 5px 10px;
    font-weight: 500;
    }
    
    .movies,
    .tv-series{
      display: flex;
      height: 100%;
      overflow-x: scroll;
      overflow-y: visible;
      scrollbar-width: none;
    }

  }

}
</style>