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

        <!-- Messaggio stampato se la ricerca non produce risultati -->
        <h2 :class="isNotFound ? 'd-block' : 'd-none'" class="text-center py-5">{{contentNotFound}}</h2>

          <div class="container-fluid">
            <!-- Movies -->
            <div class="title-content">Movies</div>
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5 mb-5">

              <CardComp 
              v-for="movieItem in arrayMovies" :key="movieItem.id"
              :movie="movieItem"
              />

            </div>
            <!-- /Movies -->

            <!-- Tv Series -->
            <div class="title-content">Tv Series</div>
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5">

              <CardComp 
              v-for="serieItem in arraySeries" :key="serieItem.id"
              :serie="serieItem"
              />

            </div>
            <!-- /Tv Series -->

          </div>
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

      contentNotFound: 'LA TUA RICERCA NON HA PRODOTTO NESSUN RISULTATO...',
      isNotFound: false
    }
  },

  methods:{
    getApi(type){
      axios.get(this.apiUrl + type, {
        params: this.apiParams
      })
      .then(resp => {
        console.log(resp.data);
        this.arrayMovies = resp.data.results;
        this.arraySeries = resp.data.results;

        // type = this.apiParams.media_type;

        this.isLoading = false;

        if(type == 'movie'){
          if(this.arrayMovies.length < 1) this.isNotFound = true;
        }else if(type == 'tv'){
          if(this.arraySeries.length < 1) this.isNotFound = true;
        }
        
      })
      .catch(error => {
        this.errorMessage = error;
        this.isError = true;
        console.log(this.errorMessage);
      })
    },

    contentSearched(inputRicerca){
      this.apiParams.query = inputRicerca.toLowerCase();
      if(inputRicerca.length > 0){
        
        this.getApi('movie');
        // console.log(this.getApi('movie'));

        this.getApi('tv');
        // console.log(this.getApi('tv'));

        this.isBlock = false;
        this.isLoading = true;
        console.log( 'sto cercando il contenuto', this.apiParams.query );
      } 
      else{
        this.isBlock = false;
        this.arrayMovies = [];
        this.isNotFound = true;
      } 

      
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
    // margin-bottom: 20px;
    }

  }

}
</style>