<template>

  <div class="col mb-4">
    <div class="movie-card text-center my-5 p-2">
      <div class="title">Titolo: {{movie.title}}</div>
      <div class="original-title">Titolo originale: {{movie.original_title}}</div>
      <div class="language">Lingua: {{movie.original_language}}</div>
      <div class="vote">Voto: {{movie.vote_average}}</div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'MoviesComp',

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