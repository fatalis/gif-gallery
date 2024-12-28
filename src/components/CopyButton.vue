<script setup lang="ts">
const props = defineProps<{
  url: string;
}>();

const copyToClipboard = async () => {
  await navigator.clipboard.writeText(props.url);
  showNotification();
};

const showNotification = () => {
  const event = new CustomEvent('show-notification', {
    detail: 'Copied URL to clipboard'
  });
  window.dispatchEvent(event);
};
</script>

<template>
  <button 
    class="copy-button"
    @click.stop="copyToClipboard"
    title="Copy image URL"
  >
    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M16 4h2a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h2"></path>
      <rect x="8" y="2" width="8" height="4" rx="1" ry="1"></rect>
    </svg>
  </button>
</template>

<style scoped>
.copy-button {
  position: absolute;
  bottom: 1rem;
  right: 1rem;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #3b82f6;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  opacity: 0;
  transform: scale(0.8);
  transition: all 0.2s ease;
  color: white;
  padding: 0;
}

.copy-button:hover {
  background-color: #2563eb;
  transform: scale(1.1);
}
</style>
