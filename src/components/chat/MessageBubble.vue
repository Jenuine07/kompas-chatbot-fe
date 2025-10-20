<template>
  <div :class="['message-bubble', message.sender]">
    <div v-html="formattedText"></div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  message: {
    type: Object,
    required: true,
  },
});

const formattedText = computed(() => {
  let text = props.message.text;

  const boldRegex = /\*\*(.*?)\*\*/g;
  text = text.replace(boldRegex, '<strong>$1</strong>');

  const markdownLinkRegex = /\[(.*?)\]\((https?:\/\/[^\s)]+)\)/g;
  text = text.replace(markdownLinkRegex, '<a href="$2" target="_blank" rel="noopener noreferrer">$1</a>');

  const rawUrlRegex = /(?<!href=")(https?:\/\/[^\s]+)/g;
  text = text.replace(rawUrlRegex, (url) => {
    if (text.includes(`href="${url}"`)) {
      return url;
    }
    return `<a href="${url}" target="_blank" rel="noopener noreferrer">${url}</a>`;
  });

  return text;
});
</script>

<style scoped>
.message-bubble.ai :deep(a) {
  color: #76c8ff; 
  text-decoration: underline;
}

.message-bubble.ai :deep(a:hover) {
  color: #a9dbff; 
}

.message-bubble :deep(strong) {
  font-weight: 700; 
}
</style>