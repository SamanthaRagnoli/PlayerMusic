<template>
    <div class="music-player">
      <MusicCard :img="current.img" 
                :title="current.title" 
                :artist="current.artist"
                :isPlaying="isPlaying" />
    
  
      <div class="bottom-section">
        <div class="progress-bar" @click="seek">
          <div class="progress" :style="{ width: progress + '%' }"></div>
        </div>
  
        <div class="time">
          <span class="current-time">{{ currentTimeFormatted }}</span>
          <span class="spacer"></span> 
          <span class="total-time">{{ remainingTimeFormatted }}</span> 
        </div>
  
        <div class="controlli">
          <div class="volume-controls">
            <img class="icon" src="/assets/icons/volume-off.svg" alt="Icona volume disattivato"> 
            <div class="volume-bar" @click="setVolume" @mousedown="startDragging" @mouseup="stopDragging" @mousemove="dragVolume">
              <div class="volume-level" :style="{ width: volumeLevel + '%' }"></div>
            </div>
            <img class="icon" src="/assets/icons/volume-up.svg" alt="Icona volume attivo">
          </div>
          <div class="playback-controls">
            <button class="prev" @click="prev">
              <img class="pcontrol" src="/assets/icons/backward.svg" alt="Indietro"> 
            </button>
            <button class="play" v-if="!isPlaying" @click="play">
              <img class="pcontrol" src="/assets/icons/play.svg" alt="Play">
            </button>
            <button class="pause" v-else @click="pause">
              <img class="pcontrol" src="/assets/icons/pause.svg" alt="Pausa">
            </button>
            <button class="next" @click="next">
              <img class="pcontrol" src="/assets/icons/forward.svg" alt="Avanti">
            </button> 
          </div>
          <button class="repeat-button" @click="toggleRepeat" :class="{ active: repeat, repeatOne: repeat === 'one' }">
            <img v-if="repeat === 'loop'" class="icon-on" src="/assets/icons/repeat.svg" alt="Ripeti in loop">
            <img v-else-if="repeat === 'one'" class="icon-on" src="/assets/icons/repeat-1.svg" alt="Ripeti una volta">
            <img v-else class="icon" src="/assets/icons/repeat.svg" alt="Ripetizione disattivata"> 
          </button>
          </div>
        </div>
      </div>
   
  </template>
  
  <script>
  import MusicCard from './MusicCard.vue';
  
  export default {
  name: 'MusicPlayer',
  components: {
    MusicCard
  },
  emits: ['updateCurrent', 'updatePlaying'],
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      isMuted: false,
      volumeLevel: 100,
      isDragging: false,
      repeat: false,
      songs: [
        {
          title: 'Ectoplasm',
          artist: 'Boom',
          src: '/assets/Music/Ectoplasm.mp3',
          img: '/assets/Images/img_prova.jpg' 
        },
        {
          title: 'GroovyBaby',
          artist: 'Grace',
          src: '/assets/Music/GroovyBaby.mp3',
          img: '/assets/Images/img_prova2.jpg' 
        },
        {
          title: 'Fly Me To The Moon',
          artist: 'Frank Sinatra',
          src: '/assets/Music/Fly Me To The Moon (2008 Remastered).mp3',
          img: '/assets/Images/frank_sinatra.jpg' 
        },
        {
          title: 'Estate',
          artist: 'Bruno Martino',
          src: '/assets/Music/Estate.mp3',
          img: '/assets/Images/bruno_martino.jpg' 
        },
        {
          title: 'Sweet Child O Mine',
          artist: 'Guns N Roses',
          src: '/assets/Music/Guns N Roses - Sweet Child O Mine (Official Music Video) .mp3',
          img: '/assets/Images/gun_n_roses.jpg' 
        },
        {
          title: 'Il Ragazzo Della Via Gluck',
          artist: 'Adriano Celentano',
          src: '/assets/Music/Il Ragazzo Della Via Gluck.mp3',
          img: '/assets/Images/adriano_celentano.jpg' 
        },
        {
          title: 'Billie Jean',
          artist: 'Michael Jackson',
          src: '/assets/Music/Michael Jackson - Billie Jean (Official Video).mp3',
          img: '/assets/Images/michael_jackson.jpg' 
        },
        {
          title: 'Se Telefonando',
          artist: 'Mina',
          src: '/assets/Music/Mina - Se telefonando (1966).mp3',
          img: '/assets/Images/mina.jpg' 
        },
        {
          title: 'Bohemian Rapsody',
          artist: 'Queen',
          src: '/assets/Music/Queen – Bohemian Rhapsody (Official Video Remastered).mp3',
          img: '/assets/Images/queen.jpg' 
        },
        {
          title: 'A Mano A Mano',
          artist: 'Rino Gaetano',
          src: '/assets/Music/Rino Gaetano - A mano a mano (Live).mp3',
          img: '/assets/Images/rino_gaetano.jpg' 
        },
        {
          title: 'APT',
          artist: 'Rosé & Bruno Mars',
          src: '/assets/Music/ROSÉ & Bruno Mars - APT. (Official Music Video).mp3',
          img: '/assets/Images/rosé_bruno_mars.jpg' 
        },
        {
          title: 'Houdini',
          artist: 'Eminem',
          src: '/assets/Music/Houdini.mp3',
          img: '/assets/Images/eminem.jpg' 
        },
        {
          title: 'Work',
          artist: 'Rihanna',
          src: '/assets/Music/Work.mp3',
          img: '/assets/Images/rihanna.jpg' 
        },
        {
          title: 'Morire X Te',
          artist: 'Mida',
          src: '/assets/Music/Mida - Morire x te (Official Video).mp3',
          img: '/assets/Images/mida.jpg' 
        },
        {
          title: 'Espresso',
          artist: 'Sabrina Carpenter',
          src: '/assets/Music/Sabrina Carpenter - Espresso (Official Audio).mp3',
          img: '/assets/Images/sabrina_carpenter.jpg' 
        },
        {
          title: 'Per Due Come Noi',
          artist: 'Olly, Angelina Mango',
          src: '/assets/Music/Olly, Angelina Mango, JVLI - Per due come noi (Official Video).mp3',
          img: '/assets/Images/olly_mango.jpg' 
        },
        {
          title: 'Die With A Smile',
          artist: 'Lady Gaga, Bruno Mars',
          src: '/assets/Music/Lady Gaga, Bruno Mars - Die With A Smile (Official Music Video).mp3',
          img: '/assets/Images/gaga_mars.jpg' 
        },
        {
          title: 'Il Filo Rosso',
          artist: 'Alfa',
          src: '/assets/Music/ALFA - il filo rosso.mp3',
          img: '/assets/Images/alfa.jpg' 
        },
        {
          title: 'Birds Of Feather',
          artist: 'Billie Eilish',
          src: '/assets/Music/Billie Eilish - BIRDS OF A FEATHER (Official Music Video).mp3',
          img: '/assets/Images/billie_eilish.jpg' 
        },
        {
          title: 'Grandine',
          artist: 'Holden',
          src: '/assets/Music/Holden - GRANDINE feat. mew (Visual Video).mp3',
          img: '/assets/Images/holden.jpg' 
        },
        {
          title: 'Rosso Relativo',
          artist: 'Tiziano Ferro',
          src: '/assets/Music/Tiziano Ferro - Rosso Relativo (Official Video).mp3',
          img: '/assets/Images/tiziano_ferro.jpg' 
        },
        {
          title: 'Unforgivable Sinner',
          artist: 'Lene Marlin',
          src: '/assets/Music/Lene Marlin - Unforgivable Sinner.mp3',
          img: '/assets/Images/lene_marlin.jpg' 
        },
        {
          title: 'Maybe Tomorrow',
          artist: 'Stereophonics',
          src: '/assets/Music/Maybe Tomorrow.mp3',
          img: '/assets/Images/stereophonics.jpg' 
        },   
        {
          title: 'We Pray',
          artist: 'Coldplay, Little Simz, Burna Boy, Elyanna, TINI',
          src: '/assets/Music/WE PRAY - Coldplay, Little Simz, Burna Boy, Elyanna, TINI (Official Audio).mp3',
          img: '/assets/Images/coldplay_others.jpg' 
        },
        {
          title: 'They Dont Care About Us',
          artist: 'Michael Jackson',
          src: '/assets/Music/They Dont Care About Us (Remastered Version).mp3',
          img: '/assets/Images/michael_jackson_2.jpg' 
        },
        {
          title: 'Mezzocielo',
          artist: 'Ilan',
          src: '/assets/Music/Mezzocielo.mp3',
          img: '/assets/Images/ilan.jpg' 
        },
        {
          title: 'Perché',
          artist: 'Chiamamifaro',
          src: '/assets/Music/Perché.mp3',
          img: '/assets/Images/chiamamifaro.jpg' 
        },
        {
          title: 'Madrid',
          artist: 'Senza Cri',
          src: '/assets/Music/MADRID.mp3',
          img: '/assets/Images/senza_cri.jpg' 
        },
        {
          title: 'Lose Yourself',
          artist: 'Eminem',
          src: '/assets/Music/Eminem - Lose Yourself.mp3',
          img: '/assets/Images/eminem_2.jpg' 
        },

        
      ],
      player: new Audio(),
      currentTime: 0,
      duration: 0,
      progress: 0
    }
  },
  computed: {
    currentTimeFormatted() {
      return this.formatTime(this.currentTime);
    },
    durationFormatted() {
      return this.formatTime(this.duration);   
   

    },
    remainingTimeFormatted() {
      const remainingTime = this.duration - this.currentTime;
      return '-' + this.formatTime(remainingTime);
    }
  },
  methods: {
    play() {
      this.isPlaying = true;
      this.player.play()
        .catch(error => {
          console.error("Errore durante la riproduzione dell'audio:", error);
          this.isPlaying = false; 
        });
    },
    pause() {
      this.isPlaying = false;
      this.player.pause();
    },
    next() {
      let newIndex;
      do {
        newIndex = Math.floor(Math.random() * this.songs.length);
      } while (newIndex === this.index && this.songs.length > 1); 

      this.index = newIndex;
      this.updateSong();
      
    },
    prev() {
      let newIndex;
      do {
        newIndex = Math.floor(Math.random() * this.songs.length);
      } while (newIndex === this.index && this.songs.length > 1);

      this.index = newIndex;
      this.updateSong();
},
    updateSong() {
      this.current = this.songs[this.index];
      this.player.src = this.current.src; 
      this.player.load(); 
      this.updateDuration();

      if (this.isPlaying) {
        this.player.play()
          .catch(error => {
            console.error("Errore durante la riproduzione dell'audio:", error);
          });
      }
      this.resetProgress();
    },
    formatTime(seconds) {
      const minutes = Math.floor(seconds / 60);
      const remainingSeconds = Math.floor(seconds % 60);
      const formattedSeconds = remainingSeconds   
  < 10 ? `0${remainingSeconds}` : remainingSeconds;
      return `${minutes}:${formattedSeconds}`;   
   

    },
    updateProgress() {
      this.currentTime = this.player.currentTime;
      this.duration = this.player.duration;
      this.progress = (this.currentTime / this.duration) * 100;
    },
    updateDuration() {
      this.duration = this.player.duration;
    },
    resetProgress() {
      this.currentTime = 0;
      this.progress = 0;
    },
    seek(event) {
      const progressBar = event.currentTarget;
      const clickX = event.offsetX;
      const progressBarWidth = progressBar.offsetWidth;
      const newPosition = (clickX / progressBarWidth) * this.player.duration;
      this.player.currentTime = newPosition;
    },
    toggleMute() {
      this.isMuted = !this.isMuted;
      this.player.muted = this.isMuted;
      this.updateVolume();
    },
    setVolume(event) {
      const volumeBar = event.currentTarget;
      const clickX = event.offsetX;
      this.volumeLevel = (clickX / volumeBar.offsetWidth) * 100; 
      this.updateVolume();
    },
    startDragging() {
      this.isDragging = true;
    },
    stopDragging() {
      this.isDragging = false;
    },
    dragVolume(event) {
      if (this.isDragging) {
        this.setVolume(event);
      }
    },
    updateVolume() {
      this.player.volume = this.volumeLevel / 100; 
      if (this.player.volume === 0) {
        this.isMuted = true;
      } else {
        this.isMuted = false;
      }
    },
    toggleRepeat() {
    if (this.repeat === false) {
        this.repeat = 'loop';
        this.player.loop = true; 
        this.player.removeEventListener('ended', this.handleSongEnd);
    } else if (this.repeat === 'loop') {
        this.repeat = 'one';
        this.player.loop = false;
        this.player.removeEventListener('ended', this.handleSongEnd);
        this.player.addEventListener('ended', this.repeatOne);
    } else {
        this.repeat = false;
        this.player.loop = false;
        this.player.removeEventListener('ended', this.repeatOne);
        this.player.addEventListener('ended', this.handleSongEnd);
    }
  },
    repeatOne() {
        this.player.currentTime = 0; 
        this.player.play().catch(error => { 
            console.error("Errore durante la riproduzione:", error); 
        }); 
        this.toggleRepeat(); 
    },
    handleSongEnd() { 
      if (this.repeat === false) {
        this.next(); 
      }
    },
  },
  mounted() {
    this.index = Math.floor(Math.random() * this.songs.length); 
    this.updateSong(); 
    const player = this.player;

    player.addEventListener('timeupdate', this.updateProgress);
    player.addEventListener('ended', this.resetProgress);
    player.addEventListener('loadedmetadata', this.updateDuration); 
    player.addEventListener('ended', this.handleSongEnd);
  },
  beforeUnmount() {
    this.player.removeEventListener('timeupdate', this.updateProgress);
    this.player.removeEventListener('ended', this.resetProgress);
    this.player.removeEventListener('loadedmetadata', this.updateDuration); 
  }
}
  </script>

<style scoped>


.bottom-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  flex-grow: 1;  
  width: 100%;
  position: fixed; 
  bottom: 0; 
  left: 0; 
  width: 100%; 
  background-color: #333; 
  padding-top: 30px; 
}

.controlli {
  display: flex;
  flex-wrap: nowrap;
  width: 50%;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  gap: 10px;
  margin-bottom: 10px;
}

.playback-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  flex: 1;
  min-width: 40px;
  
}

.volume-controls, .repeat-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  flex: 1;
  min-width: 40px;

}

.controlli button {
  margin: 0;
  padding: 6px 12px; 
  background-color: transparent;
  border: none; 
  cursor: pointer; 
  
}

.pcontrol {
  width: 25px; 
  height: 25px;
  
}

.icon {
  width: 14px; 
  height: 14px;
  opacity: 50%;
}

.icon-on {
  width: 14px; 
  height: 14px;
  opacity: 100%;
}


.progress-bar {
  width: 85%;
  height: 4px;
  background-color: #6a6a6a;
  border-radius: 5px;
  margin: 5px 0;
}

.progress {
  height: 85%;
  background-color: #ccc; 
  border-radius: 5px;
}


.time {
  display: flex;
  justify-content: space-between;
  align-items: center; 
  margin: 2px 0;
  font-size: 10px;
  width: 85%; 
  opacity: 50%;
}

.time .spacer {
  flex-grow: 1; 
}

.volume-controls {
  display: flex; 
  align-items: center;
}

.volume-bar {
  width: 60px; 
  height: 4px; 
  background-color: #777;
  border-radius: 5px;
  cursor: pointer;
  margin: 0 5px;
  display: block;
}

.volume-level {
  height: 100%;
  background-color: #ccc;
  border-radius: 5px;
}

.repeat-button {
  align-items: center;
}

.repeat-button.active { 
  fill: white; 
}

.repeat-button.active { 
  fill: white; 
}

.repeat-button.repeatOne { 
  fill: red; 
}

.music-player { 
  width: 80%;
 
}

/* Media Queries for different screen sizes */

/* Styles for tablets and larger screens */
@media (min-width: 768px) {
.bottom-section {
    padding: 20px;
  }

.controlli {
    gap: 10px;
    flex-wrap: wrap;
  }

.controlli button {
    padding: 8px 16px;
  }

  .playback-controls,
  .volume-controls,
  .repeat-button {
    flex: 0 0 auto; /* Riporto a dimensione fissa per schermi grandi */
  }

.pcontrol {
    width: 30px;
    height: 30px;
  }

.icon,.icon-on {
    width: 15px;
    height: 15px;
  }

.progress-bar {
    height: 6px;
    margin: 7px 0;
  }

.time {
    font-size: 12px;
  }

.volume-bar {
    width: 80px;
    height: 6px;
    margin: 0 10px;
  }
}

/* Styles for phones */
@media (max-width: 767px) {


.playback-controls,
.volume-controls,
.repeat-button {
    
    min-width: 150px; /* Remove minimum width */
    margin-bottom: 8px; /* Slightly reduced margin */
  }

.controlli button {
    margin: 12 8px; /* Slightly reduced button margins */
  }

.pcontrol {
    width: 30px; /* Slightly larger buttons on very small screens if needed */
    height: 30px; /* Slightly larger buttons on very small screens if needed */
  }

.volume-bar {
    width: 50px; /* Further reduced volume bar width */
  }
}
</style>