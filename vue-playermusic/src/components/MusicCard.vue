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
  margin-bottom: 130px;
  text-align: left;
}

.song-img {
  display: block;
  width: 50%;
  max-width: 430px;
  height: auto;
  border-radius: 50%;
  box-shadow: 0px 3px 12px rgba(0, 0, 0, 0.5);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  animation: rotate 10s linear infinite; 
  animation-play-state: paused; 
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

.song-info { 
  margin-left: 10%;
  
}

.song-title {
  font-family: Poppins;
  font-size: 35px;
  line-height: 1.1;
  align-items: center;
  
}

.song-artist {
  font-family: Poppins;
  font-size: 20px; 
  color: #888; 
  
}


@media (max-width: 1024px) {
  
  .song-card {
  display: flex;
  align-items: center;
  flex-direction: column;
  width: 100%;
  text-align: center;
  margin-bottom: 120px;

}

.song-img {
  display: block;
  width: 60%;
  max-width: 300px;
  align-items: center;
  margin-bottom: 50px;
  margin-left: 0px;
}


.song-title {
  font-family: Poppins;
  font-size: 30px;
  line-height: 1.1;
  align-items: center;
  
}

.song-artist {
  font-family: Poppins;
  font-size: 22px; 
  color: #888; 
  align-items: center;
  
}

.song-info { 
  margin-left: 0;
  
}

}





@media (max-width: 767px) { /* Larghezza massima di 767px (puoi cambiarla) */
  .song-card {
  display: flex;
  align-items: center;
  flex-direction: column;
  width: 100%;
  text-align: center;
  margin-bottom: 110px;

}

.song-img {
  display: block;
  width: 80%;
  max-width: 250px;
  border-radius: 50%;
  align-items: center;
  margin-bottom: 50px;
  margin-left: 0px;
}

.song-title {
  font-family: Poppins;
  font-size: 28px;
  line-height: 1.1;
  align-items: center;
  text-align: center;
  
}

.song-artist {
  font-family: Poppins;
  font-size: 18px; 
  color: #888; 
  align-items: center;
  
}


.song-info { 
  margin-left: 0;
  
}

}





</style>