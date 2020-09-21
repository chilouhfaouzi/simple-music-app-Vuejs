<template>
  <header>
    <h1>My Music</h1>
  </header>
  <main>
    <section>
      <h2 class="song-title">
        {{ current.title }} - <span> {{ current.artist }}</span>
      </h2>
      <div class="control">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">
          Pause
        </button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>
    <section class="playlist">
      <h2>My Playlist</h2>
      <button
        v-for="song in songs"
        :key="song.src"
        @click="play(song)"
        :class="current.title == song.title ? 'song playing' : 'song'"
      >
        {{ song.title }} - {{ song.artist }}
      </button>
    </section>
  </main>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "sufi",
          artist: "name 1",
          src: require("./assets/piano.mp3"),
        },
        {
          title: "piano",
          artist: "name 2",
          src: require("./assets/sufi.mp3"),
        },
      ],
      player: new Audio(), //element of html5
    };
  },
  methods: {
    play(song) {
      if (typeof song.title != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play(); //methods on html5 dom
      this.isPlaying = true;
    },
    pause() {
      this.player.pause(); //methods on html5 dom
      this.isPlaying = false;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
}
header {
  background-color: rgba(235, 72, 44, 0.74);
  text-align: center;
  color: #fff;
  max-width: 100%;
  width: 700px;
  margin: 0 auto;
}
main {
  text-align: center;
  padding: 30px;
  max-width: 100%;
  width: 700px;
  margin: 0 auto;
}
.song-title {
  font-size: 25px;
  font-weight: bold;
  text-transform: uppercase;
}
.song-title span {
  font-size: 22px;
  font-style: italic;
}
.control {
  padding: 30px 0;
}
.control button {
  background-color: transparent;
  border: none;
  margin: 0 20px;
  cursor: pointer;
}
.play,
.pause {
  background-color: rgb(243, 47, 87) !important;
  color: #fff;
  font-size: 25px;
  font-weight: bold;
  border-radius: 10px;
  padding: 5px 15px;
}
.prev,
.next {
  background-color: rgba(241, 77, 55, 0.856) !important;
  color: #fff;
  font-size: 20px;
  border-radius: 8px;
  padding: 2px 10px;
}

.playlist {
  padding: 20px 0;
}

.playlist .song {
  display: block;
  background-color: transparent;
  border: none;
  padding: 10px 20px;
  margin: 15px auto;
  font-size: 18px;
  width: 90%;
  cursor: pointer;
}
.playlist .song:not(.playing):hover {
  color: rgba(238, 66, 43, 0.856);
  font-size: 1.1em;
}

.playlist .playing {
  background-image: linear-gradient(
    to right,
    rgba(238, 66, 43, 0.856),
    rgb(243, 47, 87)
  );
  color: #fff;
  font-size: 24px;
  border-radius: 5px;
}
</style>
