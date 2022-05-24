<template>

  <div class="col mb-4">
    <div class="movie-card my-2 p-2">    
      <img class="mb-4" :src="'https://image.tmdb.org/t/p/w200/'+item.backdrop_path" alt=""> 
      <div class="title mb-4">Titolo: {{changeTitleName}}</div>  
      <div class="original-title mb-4">Titolo originale: {{changeOriginalTitleName}}</div>
      <div class="language mb-4"><lang-flag :iso="item.original_language" /></div> 

      <!-- Voto stelle -->

      <star-rating v-model="changeStarVote" class="vote mb-4"
      v-bind:increment="0.5"
      v-bind:max-rating="5"
      inactive-color="#000"
      active-color="#DB202C"
      v-bind:star-size="20"></star-rating>

      <div class="descrizione">Descrizione: {{item.overview}}</div>

      <img :src="'https://image.tmdb.org/t/p/w342/'+item.poster_path" alt="">
    </div>

    <!-- ANIMAZIONE CARD DA SISTEMARE -->
    <!-- <div class="flip-card">
      <div class="flip-card-inner">
        <div class="flip-card-front">
          <img :src="'https://image.tmdb.org/t/p/w200/'+item.poster_path" alt="">
        </div>
        <div class="flip-card-back">
          <div class="title mb-4">Titolo: {{changeTitleName}}</div>  
          <div class="original-title">Titolo originale: {{changeOriginalTitleName}}</div>
          <div class="language mb-4"><lang-flag :iso="item.original_language" /></div> 

          <div class="vote mb-4">Voto: {{item.vote_average}}</div>

          <div class="descrizione">Descrizione: {{item.overview}}</div>

        </div>
      </div>
    </div>  -->

  </div>

</template>

<script>
import LangFlag from 'vue-lang-code-flags';
import StarRating from 'vue-star-rating'

export default {
  name: 'CardComp',

  components:{
    LangFlag,
    StarRating
  },

  props:{
    item: Object,
  },

  computed:{
    changeTitleName(){
      if(this.item.title != undefined){
        return this.item.title
      }else{
        return this.item.name
      }
      
    },
    changeOriginalTitleName(){
      if(this.item.original_title != undefined){
        return this.item.original_title
      }else{
        return this.item.original_name
      }
    },

    changeStarVote(){
      let starVote = Math.round(this.item.vote_average) / 2;

      return starVote;
    }
  },

}
</script>

<style lang="scss" scoped>
.flip-card {
  background-color: transparent;
  // width: 300px;
  // height: 200px;
  min-height: 350px;
  font-size: .8rem;
  // border: 1px solid #f1f1f1;
  perspective: 1000px; /* Remove this if you don't want the 3D effect */
}

/* This container is needed to position the front and back side */
.flip-card-inner {
  position: relative;
  // width: 100%;
  // height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front, .flip-card-back {
  position: absolute;
  // width: 100%;
  // height: 100%;
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
  // background-color: #bbb;
  color: black;
}

/* Style the back side */
.flip-card-back {
  background-color: rgba(97, 95, 95, 0.561);
  // color: white;
  transform: rotateY(180deg);
}

.movie-card{
  background-color: rgba(97, 95, 95, 0.561);
  height: 100%;
  font-size: .8rem;
  transition: all .2s;
  border-radius: 5px;
  cursor: pointer;  
}

.language{
  font-size: 1.5rem;
}

.descrizione{
  height: 150px;
  overflow-y: scroll;
}


</style>