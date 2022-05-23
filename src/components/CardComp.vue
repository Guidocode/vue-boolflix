<template>

  <div class="col mb-4">
    <div class="movie-card my-5 p-2">
      <div class="title mb-4">Titolo: {{movie.title}}</div>                          <!-- .name -->
      <div class="original-title">Titolo originale: {{movie.original_title}}</div>   <!-- .original_name -->
      <div class="language mb-4"><lang-flag :iso="movie.original_language" /></div>   
      <div class="vote mb-4">Voto: {{movie.vote_average}}</div>
      <div class="descrizione">Descrizione: {{movie.overview}}</div>
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
    movie: Object,
    serie: Object
  },

  methods:{

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