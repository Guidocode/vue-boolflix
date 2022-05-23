<template>

  <div class="col mb-4">
    <div class="movie-card my-2 p-2">     
      <div class="title mb-4">Titolo: {{changeTitleName}}</div>  
      <div class="original-title">Titolo originale: {{changeOriginalTitleName}}</div>
      <div class="language mb-4"><lang-flag :iso="item.original_language" /></div> 
      <div class="vote mb-4">Voto: {{item.vote_average}}</div>
      <div class="descrizione">Descrizione: {{item.overview}}</div>
    </div>
  </div>

</template>

<script>
import LangFlag from 'vue-lang-code-flags';

export default {
  name: 'CardComp',

  components:{
    LangFlag
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
    }
  }
}
</script>

<style lang="scss" scoped>
.movie-card{
  background-color: rgba(97, 95, 95, 0.561);
  height: 100%;
  font-size: .8rem;
  transition: all .2s;
  border-radius: 5px;
  cursor: pointer;

  .language{
    font-size: 1.5rem;
  }

  .descrizione{
    height: 150px;
    overflow-y: scroll;
  }
}
</style>