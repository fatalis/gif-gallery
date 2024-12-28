<script setup lang="ts">
import { ref, onMounted } from 'vue';
import type { Image } from '../types/image';
import GalleryItem from './GalleryItem.vue';
import * as ThumbHash from 'thumbhash';

let images = ref<Image[]>([]);

type Gif = [qs: string, expiration: number, sha1: string, mtime: number, width: number, height:number, chan: string, uid: string, placeholder: string];
type GifCollection = {
  [fn: string]: Gif
}

onMounted(async () => {
  const response = await fetch('https://api.gifs.fatalis.pw');
  const obj: GifCollection = await response.json();
  for (const [fn, gif] of Object.entries(obj)) {
    // @ts-ignore
    const [qs, expiration, sha1, mtime, width, height, chan, uid, placeholder] = gif;
    let fnn: string;
    if (fn.endsWith('.webp')) {
      fnn = fn.replace('.webp', '.th.webp');
    } else {
      fnn = fn.replace('.gif', '.th.webp');
    }
    const path = 'https://r2.gifs.fatalis.pw/' + fnn;
    const hq_path = `https://cdn.discordapp.com/attachments/${chan}/${uid}/${fn}${qs}`;
    const base64ToBinary = (base64: string) => new Uint8Array(atob(base64).split('').map(x => x.charCodeAt(0)));
    const thumbHashFromBase64 = base64ToBinary(placeholder);
    const ph = ThumbHash.thumbHashToDataURL(thumbHashFromBase64);

    images.value.push({
      path,
      hq_path,
      width,
      height,
      mtime,
      placeholder: ph,
    });
  }

  images.value.sort((a, b) => b.mtime - a.mtime);
});

</script>

<template>
  <div class="gallery">
    <GalleryItem
      v-for="image in images"
      :key="image.path"
      :image="image"
    />
  </div>
</template>

<style scoped>
.gallery {
  column-count: 4;
  column-gap: 1rem;
  padding: 1rem;
  max-width: 1400px;
  margin: 0 auto;
}

@media (max-width: 1200px) {
  .gallery {
    column-count: 3;
  }
}

@media (max-width: 768px) {
  .gallery {
    column-count: 2;
  }
}

@media (max-width: 480px) {
  .gallery {
    column-count: 1;
  }
}
</style>
