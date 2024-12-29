<script setup lang="ts">
import ImageGallery from './components/ImageGallery.vue'
import Notification from './components/Notification.vue'
</script>

<template>
  <div class="app-container">
    <div style="display: flex; justify-content: center; margin: 0 2rem 0 2rem">
      <img src="/yappie3.png">
      <h1>itsKatchii GIFs</h1>
    </div>
    <small>not all gifs are uploaded yet</small>
    <ImageGallery />
    <Notification />
  </div>
</template>

<style>
.app-container {
  width: 100%;
  min-height: 100vh;
  /* padding: 2rem; */
}

h1 {
  text-align: center;
  margin-bottom: 2rem;
  color: #d294df;
  padding-top:1rem;
  padding-left: 2rem;
}
</style>
