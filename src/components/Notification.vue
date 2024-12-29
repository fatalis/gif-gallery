<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const isVisible = ref(false);
const message = ref('');

const showNotification = (event: CustomEvent) => {
  message.value = event.detail;
  isVisible.value = true;
  setTimeout(() => {
    isVisible.value = false;
  }, 2000);
};

onMounted(() => {
  window.addEventListener('show-notification', showNotification as EventListener);
});

onUnmounted(() => {
  window.removeEventListener('show-notification', showNotification as EventListener);
});
</script>

<template>
  <Transition name="fade">
    <div v-if="isVisible" class="notification">
      {{ message }}
    </div>
  </Transition>
</template>

<style scoped>
.notification {
  position: fixed;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  background-color: #1f2937;
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 9999px;
  z-index: 5000;
  box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(1rem);
}
</style>
