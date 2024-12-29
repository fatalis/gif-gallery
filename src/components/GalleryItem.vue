<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import type { Image } from '../types/image';
import CopyButton from './CopyButton.vue';

const props = defineProps<{
  image: Image;
}>();

const ready = ref(false);
const showLightbox = ref(false);

const handleImageClick = () => {
  showLightbox.value = true;
  document.body.style.overflow = 'hidden';
};

const closeLightbox = () => {
  showLightbox.value = false;
  document.body.style.overflow = '';
};

const handleKeydown = (e: KeyboardEvent) => {
  if (e.key === 'Escape' && showLightbox.value) {
    closeLightbox();
  }
};

onMounted(() => {
  window.addEventListener('keydown', handleKeydown);
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown);
});

const load = (event:any) => {
  if (event.target && event.target.src !== props.image.hq_path) {
    event.target.src = props.image.hq_path;
  }
};
</script>

<template>
  <div class="gallery-item" @contextmenu.prevent @click.prevent="handleImageClick"
    :style="`aspect-ratio: ${Math.max(image.width / image.height, 0.7)}`">
    <a :href="image.hq_path" target="_blank">
      <img @load="ready = true" :src="ready ? image.path : image.placeholder" alt="" loading="lazy"
        :data-src="image.path">
    </a>
    <CopyButton :url="image.hq_path" />
  </div>

  <!-- Lightbox -->
  <Teleport to="body">
    <div v-if="showLightbox" class="lightbox" @click="closeLightbox">
      <div class="lightbox-content"
        :style="`aspect-ratio: ${image.width / image.height}`">
        <img :src="image.placeholder" :data-src="image.hq_path" @load="load($event)" alt="">
        <!-- <button class="close-button" @click.stop="closeLightbox">×</button> -->
      </div>
      <CopyButton style="opacity: 1; right: unset" :url="image.hq_path" />
    </div>
  </Teleport>
</template>

<style scoped>
.gallery-item {
  break-inside: avoid;
  margin-bottom: 1rem;
  /* background: #f0f0f0; */
  border-radius: 8px;
  overflow: hidden;
  position: relative;
  cursor: pointer;
  column-width: 300px;
}

.gallery-item img {
  width: 100%;
  height: 100%;
  display: block;
  transition: transform 0.3s ease;
  object-fit: cover;
  object-position: top;
}

.gallery-item:hover img {
  transform: scale(1.05);
}

.gallery-item:hover :deep(.copy-button) {
  opacity: 1;
  transform: scale(1);
}

@media (hover: none) {
  .gallery-item:hover :deep(.copy-button) {
    opacity: 0;
  }
}

.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.lightbox-content {
  position: relative;
  width: 75vw;
  height: 75vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.lightbox-content img,
.lightbox-content .close-button {
  pointer-events: auto;
}

.lightbox-content img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.close-button {
  position: absolute;
  top: -40px;
  right: -40px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.2);
  border: none;
  color: white;
  font-size: 24px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.2s;
}

.close-button:hover {
  background-color: rgba(255, 255, 255, 0.3);
}
</style>
