<template>

  <div class="movie text-center my-5">
    <h3>Titolo: {{movie.title}}</h3>
    <h5>Titolo originale: {{movie.original_title}}</h5>
    <span>Lingua: {{movie.original_language}}</span><br>
    <span>Voto: {{movie.vote_average}}</span>
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

</style>