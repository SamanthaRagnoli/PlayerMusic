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
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

#app {
  max-width: 80%; 
  max-height: 1200px;
  margin: 0 auto; 
  padding: 20px; 
}



</style>
