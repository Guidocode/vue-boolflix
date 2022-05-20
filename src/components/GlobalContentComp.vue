<template>
  <div class="global-content">

    <!-- Content Top -->
    <div class="content-top d-flex justify-content-between align-items-center px-3">

      <div class="logo">Boolflix</div>

      <div class="searchbars">
        <SeachbarComp />
      </div>
    </div>
    <!-- /Content Top -->



    <!-- Content Bottom -->
    <!-- /Content Bottom -->

  </div>
</template>

<script>
import SeachbarComp from './SeachbarComp.vue';
import axios from 'axios';
export default {
  name: "GlobalContentComp",
  components: { 
    SeachbarComp 
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
      }
    }
  },

  methods:{
    getApi(){
      axios.get(this.apiUrl, {
        params: this.apiParams
      })
      .then(resp => {
        console.log(resp.data);
      })
      .catch(error => {
        console.log(error);
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.global-content{
  height: 100vh;
  background-color: #212323;
  color: white;

  .content-top{
    min-height: 60px;
    background-image: linear-gradient(#1a2727, #2b3f3f);
  }
}
</style>