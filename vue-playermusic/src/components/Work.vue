<template>
  <div class="work">
    <div class="container">
    <MusicPlayer @update-current="updateCurrent" @update-playing="updatePlaying" /> 
    <Info/>
    </div>
  </div>
</template>

<script>
import MusicPlayer from './MusicPlayer.vue';
import MusicCard from './MusicCard.vue';
import Info from './Info.vue';

export default {
  name: 'Work',
  components: {
    MusicPlayer,
    MusicCard,
    Info
  },
  data() {
    return {
      current: {}, // Inizializza current in Work.vue
      isPlaying: false,
    };
  },
  methods: {
    updateCurrent(newCurrent) {
      this.current = newCurrent;
    },
    updatePlaying(newIsPlaying) {
      this.isPlaying = newIsPlaying;
    }
  }
}
</script>

<style scoped>
.container{
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}
</style>

