<template>
  <div class="chat-window" ref="chatWindowRef">
    <div class="message-container">
      <MessageBubble
        v-for="msg in messages"
        :key="msg.id"
        :message="msg"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, watch, nextTick } from 'vue';
import MessageBubble from './MessageBubble.vue';

const props = defineProps({
  messages: {
    type: Array,
    required: true,
  },
});

const chatWindowRef = ref(null);

watch(
  () => props.messages,
  async () => {
    await nextTick();
    if (chatWindowRef.value) {
      chatWindowRef.value.scrollTop = chatWindowRef.value.scrollHeight;
    }
  },
  { deep: true }
);
</script>