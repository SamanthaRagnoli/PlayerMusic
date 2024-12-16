<template>
   <div class="song-card">
    <div class="song-img" :style="{ backgroundImage: `url(${img})`, 'animation-play-state': isPlaying ? 'running' : 'paused' }"></div>
    <div class="song-info"> 
      <h2 class="song-title">{{ title }}</h2> 
      <p class="song-artist">{{ artist }}</p> 
    </div>
  </div>
</template>

<script>
export default {
  name: 'MusicCard',
  props: {
    img: String,
    title: String,
    artist: String,
    isPlaying: Boolean
  }
}
</script>

<style scoped>
.song-card {
  display: flex;
  align-items: center;
  width: 100%;
  margin-bottom: 150px;
}

.song-img {
  display: block;
  width: 25%;
  max-width: 100%;
  border-radius: 50%;
  box-shadow: 0px 3px 12px rgba(0, 0, 0, 0.5);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  animation: rotate 10s linear infinite; 
  animation-play-state: paused; /* Inizialmente in pausa */
  margin-left: 80px;
}

.song-img::after {
  content: '';
  display: block;
  padding-top: 100%; 
}

@keyframes rotate { 
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.song-info { /* Aggiunto div per contenere titolo e artista */
  margin-left: 10%;
  /* Centra il testo */
}

.song-title {
  font-family: Poppins;
  font-size: 35px;
  line-height: 1.1;
  
}

.song-artist { /* Stile per il nome dell'artista */
  font-family: Poppins;
  font-size: 20px; 
  color: #888; /* Colore grigio */
  
}
</style>