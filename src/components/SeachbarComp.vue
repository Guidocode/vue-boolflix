<template>
  
  <div class="input-group d-flex flex-wrap">
    <input v-model.trim="inputRicercaFilm"
    @keyup.enter="$emit('search', inputRicercaFilm)"
    type="text" class="form-control" placeholder="Scrivi il nome del film" aria-label="Recipient's username" aria-describedby="basic-addon2">
    <div class="input-group-append">
      <button @click="$emit('search', inputRicercaFilm)"
      class="btn btn-warning" type="button">Cerca</button>
    </div>
</div>

</template>

<script>
export default {
 name: 'SearchbarComp',

 data(){
   return{
    inputRicercaFilm: ''
   }
 }
}
</script>

<style lang="scss" scoped>
.input-group{
  max-width: 300px;
  margin: 10px 0 10px 5px;
}
</style>