<script setup lang="ts">
import { ref } from 'vue'
import type { Image } from '../types/image';
import CopyButton from './CopyButton.vue';

const props = defineProps<{
  image: Image;
}>();

const ready = ref(false);

// const image = props.image;

const handleImageClick = async () => {
  await navigator.clipboard.writeText(props.image.hq_path);
  const event = new CustomEvent('show-notification', {
    detail: 'Copied URL to clipboard'
  });
  window.dispatchEvent(event);
};

</script>

<template>
  <div class="gallery-item" @contextmenu.prevent @click.prevent="handleImageClick"
      :style="`aspect-ratio: ${Math.max(image.width / image.height, 0.7)}`"
    >
    <a :href="image.hq_path" target="_blank">
      <img @load="ready = true" :src="ready ? image.path : image.placeholder" alt="" loading="lazy" :data-src="image.path">
    </a>
    <CopyButton :url="image.hq_path" />
  </div>
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
</style>
