<template>

 <!-- LE CARD SCORRONO -->
  <div class="flip-card flex-shrink-0">
    <div class="flip-card-inner">

      <!-- FRONT -->
      <div class="flip-card-front">
        <img v-if="item.poster_path != '' && item.poster_path != null" 
        :src="'https://image.tmdb.org/t/p/w342/'+item.poster_path" alt="">
        <div class="text-poster-none d-flex justify-content-center align-items-center" v-else>
          <h3>{{changeTitleName}}</h3>
        </div>
      </div>
      <!-- /FRONT -->

      <!-- BACK -->
      <div class="flip-card-back">
        <div class="title mb-3">Titolo: {{changeTitleName}}</div>  
        <div :class="isBlock ? 'd-block' : 'd-none'" class="original-title mb-3">Titolo originale: {{changeOriginalTitleName}}</div>
        <div class="language mb-1"><lang-flag :iso="item.original_language" /></div> 
        <star-rating v-model="changeStarVote" class="vote mb-3"
        v-bind:increment="0.5"
        v-bind:max-rating="5"
        inactive-color="#000"
        active-color="#DB202C"
        v-bind:star-size="20">
        </star-rating>
        <div :class="item.overview != '' ? 'd-block' : 'd-none'" class="descrizione">{{item.overview}}</div>
      </div>
      <!-- /BACK -->
      
    </div> 
  </div>

  

</template>

<script>
import LangFlag from 'vue-lang-code-flags';
import StarRating from 'vue-star-rating';

export default {
  name: 'CardComp',

  data(){
    return{
      isBlock: true
    }
  },

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
        return this.item.title;
      }else{
        return this.item.name;
      }
      
    },
    changeOriginalTitleName(){
      if(this.item.original_title != undefined){
        return this.item.original_title;

        // if(this.item.original_title != this.item.title){
        //   return this.item.original_title;
        // }else{
        //   this.isBlock = false; 
        //   return '';
        // }
        
      }else{
        return this.item.original_name;

        // if(this.item.original_name != this.item.name){
        //   return this.item.original_name;
        // }else{
        //   this.isBlock = false; 
        //   return ''; 
        // }
        
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
  width: 342px;
  margin-right: 20px;
  height: 458px;
  font-size: .8rem;
  perspective: 1000px; /* Remove this if you don't want the 3D effect */
}

/* This container is needed to position the front and back side */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front, .flip-card-back {
  width: 100%;
  height: 112%;
  position: absolute;
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
}

/* Style the back side */
.flip-card-back {
  background-color: rgba(97, 95, 95, 0.561);
  padding: 10px;
  transform: rotateY(180deg);
}


.text-poster-none{
  width: 100%;
  height: 100%;
  background-color: rgba(97, 95, 95, 0.561);
}

.title,
.original-title{
  font-size: 1.3rem;
}

.language{
  font-size: 1.5rem;
}

.descrizione{
  height: 150px;
  overflow-y: scroll;
}


</style>