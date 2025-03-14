<template>
  <div class="music-navigation">
    <button class="win98-button nav-button" @click="prevAlbum">&lt;</button>
    <div class="win98-inset music-item">
      <img :src="currentAlbum.image" :alt="currentAlbum.title" class="music-image">
      <div class="album-info">
        <div class="album-title">{{ currentAlbum.title }}</div>
        <div class="album-artist">{{ currentAlbum.artist }}</div>
        <div class="album-links">
          <a :href="currentAlbum.link" class="win98-link" target="_blank">Listen on Spotify</a>
        </div>
      </div>
    </div>
    <button class="win98-button nav-button" @click="nextAlbum">&gt;</button>
  </div>
</template>

<script>
export default {
  props: {
    albums: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      currentIndex: 0
    };
  },
  computed: {
    currentAlbum() {
      return this.albums[this.currentIndex];
    }
  },
  methods: {
    nextAlbum() {
      if (this.currentIndex < this.albums.length - 1) {
        this.currentIndex++;
      } else {
        this.currentIndex = 0;
      }
    },
    prevAlbum() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
      } else {
        this.currentIndex = this.albums.length - 1;
      }
    }
  }
};
</script>

<style scoped>
.album-info {
  margin-top: 10px;
}

.album-title {
  font-weight: bold;
  margin-bottom: 5px;
}

.album-artist {
  font-style: italic;
  margin-bottom: 8px;
}

.album-links {
  margin-top: 8px;
}

.win98-link {
  color: #0000ff;
  text-decoration: underline;
  cursor: pointer;
}

.win98-link:hover {
  color: #ff0000;
}

.music-image {
  max-width: 100%;
  height: auto;
  max-height: 200px;
  display: block;
  margin: 0 auto;
  border: 1px solid var(--win98-button-shadow);
}
</style>