<template>
  <div class="albums-container">
    <h1>Explore Albums</h1>
    <div class="album-list">
      <div v-for="album in albums" :key="album.id" class="album-item">
        <router-link :to="`/albums/${album.id}`">
          <div class="album-thumbnail">
            <img :src="album.thumbnailUrl" alt="Album Cover" />
          </div>
          <div class="album-details">
            <h3>{{ album.title }}</h3>
            <p>{{ album.description }}</p>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, computed } from 'vue';
import { useAlbumStore } from '../stores/albumStore';

const albumStore = useAlbumStore();

onMounted(() => {
  albumStore.fetchAlbums();
});

const albums = computed(() => albumStore.albums);
</script>

<style scoped>
.albums-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.album-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.album-item {
  background-color: #ffffff;
  border: 1px solid #ddd;
  border-radius: 5px;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.album-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.album-item a {
  text-decoration: none;
  color: inherit;
  display: block;
}

.album-thumbnail {
  width: 100%;
  height: 150px;
  overflow: hidden;
}

.album-thumbnail img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.album-details {
  padding: 10px;
}

.album-details h3 {
  margin-top: 0;
  margin-bottom: 10px;
  font-size: 18px;
  color: #333333;
}

.album-details p {
  font-size: 14px;
  color: #666666;
  line-height: 1.4;
}
</style>
