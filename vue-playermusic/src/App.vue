<script setup>
import Work from './components/Work.vue'


</script>

<template>
  <div id="app">
    <Work />
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box; 
  
  
}

body {
  font-family: sans-serif;
}

#app {
  max-width: 100%; /* Rimuovi la larghezza massima */
  max-height: 100%;
  margin: 30px; /* Rimuovi i margini */
  padding: 0; /* Rimuovi il padding */
}



</style>
