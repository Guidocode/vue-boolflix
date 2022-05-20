<template>
  <div class="global-content">

    <!-- Content Top -->
    <div class="content-top d-flex justify-content-between align-items-center px-3">

      <div class="logo">B o o l f l i x</div>

      <div class="searchbars">
        <SeachbarComp />
      </div>

    </div>
    <!-- /Content Top -->



    <!-- Content Bottom -->
    <MoviesComp 
    v-for="movieItem in arrayMovies" :key="movieItem.id"
    :movie="movieItem"
    @search="movieSearched"
    />
    <!-- /Content Bottom -->

  </div>
</template>

<script>
import SeachbarComp from './SeachbarComp.vue';
import axios from 'axios';
import MoviesComp from './MoviesComp.vue';
export default {
  name: "GlobalContentComp",
  components: {
    SeachbarComp,
    MoviesComp
},

  mounted(){
    this.getApi();
  },

  data(){
    return{
      apiUrl: 'https://api.themoviedb.org/3/search/movie',
      apiParams: {
        api_key: '331c13478db6d90df46ce36b05f18e36',
        language: 'it-IT',
        query: 'harry potter'
      },
      arrayMovies: [],
      movieSearchedString: ''
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
      })
      .catch(error => {
        console.log(error);
      })
    }
  },

  movieSearched(inputRicercaFilm){
    if(this.arrayMovies.includes(inputRicercaFilm.toLowerCase())) this.movieSearchedString = inputRicercaFilm.toLowerCase();
  }
}
</script>

<style lang="scss" scoped>
.global-content{
  min-height: 100vh;
  background-color: #212323;
  color: white;

  .content-top{
    min-height: 60px;
    background-image: linear-gradient(#1a2727, #2b3f3f);

    .logo{
      color: #DB202C;
      font-size: 2rem;
    }
  }
}
</style>