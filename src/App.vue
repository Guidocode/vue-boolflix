<template>
  <div>
    
    <GlobalContentComp />

  </div>
</template>

<script>
import GlobalContentComp from './components/GlobalContentComp.vue';


export default {
  name: 'App',
  components: {
    GlobalContentComp
  }
}
</script>

<style lang="scss">
@import './assets/style/general'
</style>
