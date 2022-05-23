<template>

  <div class="col mb-4">
    <div class="movie-card text-center my-5 p-2">
      <h4 class="title mb-4">Titolo: {{movie.title}}</h4>
      <h5 class="original-title">Titolo originale: {{movie.original_title}}</h5>
      <div class="language"><lang-flag :iso="movie.original_language" /></div>   
      <div class="vote">Voto: {{movie.vote_average}}</div>
    </div>
  </div>

</template>

<script>
import LangFlag from 'vue-lang-code-flags';

export default {
  name: 'MoviesComp',

  components:{
    LangFlag
  },

  props:{
    movie: Object
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
}
</style>