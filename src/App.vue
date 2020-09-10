<template>
  <div id="app">
    <header>
      <div class="control">
          <button class="prev" @click="prev"></button>
          <button class="play" v-if="!isPlaying" @click="play"></button>
          <button class="pause" v-else @click="pause"></button>
          <button class="next" @click="next"></button>
        </div>
      <div class="current-track">
        <span class="now-playing">Now playing:</span> {{ current.title }} - <span>{{ current.artist }}</span></div>
    </header>
    <main>
      <section class="player">
        <div class="cover">
          <img class="cover-image" :src="trackCover">
        </div>
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
      </section>
      <section class="playlist">
        <h3>Playlist</h3>
        <button v-for="(song, index) in songs" :key="song.src" @click="play(song)" 
          :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ index + 1}}. {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 2,
      isPlaying: false,
      songs: [
        {
          title: 'Danse Macabre',
          artist: 'Camille Saint-Saëns',
          src: require('./assets/Camille Saint-Saëns - Danse Macabre.mp3')
        },
        {
          title: 'Anitras Dance',
          artist: 'Edvard Grieg',
          src: require('./assets/Edvard Grieg - Anitras Dance.mp3')
        },
        {
          title: 'Dance of the Sugar Plum Fairy',
          artist: 'Pyotr Ilyich Tchaikovsky',
          src: require('./assets/Pyotr Ilyich Tchaikovsky - Dance of the Sugar Plum Fairy.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
        this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
  computed: {
    trackCover() {
      const fileName = this.current.title.toLowerCase();
      return require(`./assets/img/covers/${fileName}.png`);
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}
body {
  font-family: sans-serif;
}  
header {
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #ffffff;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.current-track {
  display: flex;
  font-size: 12px;
  font-weight: 200;
  margin: 0 auto;
  justify-content: center;
}

.now-playing {
  margin-right: 10px;
}
.song-title {
  color: #727272;
  font-size: 22px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
  margin-top: 20px;
}

.song-title span {
  font-weight: 200;
}

.cover-image {
  display: flex;
  justify-content: center;
  width: 250px;
  height: 250px;
  margin: 0 auto;
}

.control {
  display: flex;
  width: 300px;
  justify-content: space-between;
  padding: 30px 15px;
  margin: 0 auto;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

.play {
  width: 40px;
  height: 40px;
  background-image: url('./assets/img/controls/play.png');
  background-size: 35px;
  background-repeat: no-repeat;
}

.pause {
  width: 40px;
  height: 40px;
  background-image: url('./assets/img/controls/pause.png');
  background-size: 35px;
  background-repeat: no-repeat;
}

.prev {
  width: 40px;
  height: 40px;
  background-image: url('./assets/img/controls/back.png');
  background-size: 35px;
  background-repeat: no-repeat;
  margin-right: 15px;
}

.next {
  width: 40px;
  height: 40px;
  background-image: url('./assets/img/controls/next.png');
  background-size: 35px;
  background-repeat: no-repeat;
}

.playlist {
  padding: 25px 10px;
  font-weight: 700;
}

.playlist .song {
  display: block;
  width: 100%;
  padding-top: 15px;
  text-align: left;
}
</style>
