<template>
  <div class="global-content d-flex flex-column">

    <!-- CONTENT TOP -->
    <!-- <div class="content-top container-fluid">
      <div class="row">
        <div class="col-12 col-sm-3">
          <div class="logo my-1">B o o l f l i x</div>
        </div>
        <div class="col-12 col-sm-9">
          <div class="searchbars d-flex justify-content-end">
            <SeachbarComp
            @search="movieSearched"
            />
            
          </div>
        </div>
      </div>
    </div> -->
    <div class="content-top d-flex justify-content-between align-items-center flex-wrap px-3">

      <div class="logo">B o o l f l i x</div>

      <div class="searchbars d-flex flex-wrap flex-md-nowrap">
        <SeachbarComp 
        @search="movieSearched"
        />
        
        <!-- Seconda searchbar -->
        
      </div>

    </div>
    <!-- /CONTENT TOP -->



    <!-- CONTENT BOTTOM -->
    <div class="content-bottom h-100">
      <div v-if="!isError" class="h-100">
        <div v-if="!isLoading" class="gb-container h-100 py-5">
          <div class="container-fluid">
            <!-- <div class="title-content">Movies</div> -->
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5">

              <MoviesComp 
              v-for="movieItem in arrayMovies" :key="movieItem.id"
              :movie="movieItem"
              />

            </div>
          </div>
        </div>
      
        <LoadingComp v-else />
      </div>

      <div v-else class="text-center py-5">{{errorMessage}}</div>
    </div>
    <!-- /CONTENT BOTTOM -->

  </div>
</template>

<script>
import SeachbarComp from './SeachbarComp.vue';
import axios from 'axios';
import MoviesComp from './MoviesComp.vue';
import LoadingComp from './LoadingComp.vue';
export default {
  name: "GlobalContentComp",
  components: {
    SeachbarComp,
    MoviesComp,
    LoadingComp
},

  mounted(){
    
  },

  data(){
    return{
      apiUrl: 'https://api.themoviedb.org/3/search/movie',
      apiParams: {
        api_key: '331c13478db6d90df46ce36b05f18e36',
        language: 'it-IT',
        query: ''
      },
      arrayMovies: [],

      errorMessage: '',
      isError: false,

      isLoading: null
    }
  },

  methods:{
    getApi(){
      axios.get(this.apiUrl, {
        params: this.apiParams
      })
      .then(resp => {
        console.log(resp.data);
        this.arrayMovies = resp.data.results;
        this.isLoading = false;
      })
      .catch(error => {
        this.errorMessage = error;
        this.isError = true;
        console.log(this.errorMessage);
      })
    },

    movieSearched(inputRicercaFilm){
      this.apiParams.query = inputRicercaFilm.toLowerCase();
      this.getApi();
      this.isLoading = true
      console.log( 'sto cercando il film', this.apiParams.query );
    },
  },

  

}
</script>

<style lang="scss" scoped>
.global-content{
  height: 100vh;
  color: white;
  

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
    margin-bottom: 20px;
    }

  }

}
</style>