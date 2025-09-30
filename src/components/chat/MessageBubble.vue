<template>
  <div :class="['message-bubble', message.sender]">
    {{ message.text }}
  </div>
</template>

<script setup>
// Menerima 'message' sebagai prop dari parent (ChatWindow)
// Prop ini berisi object: { id, text, sender: 'user' | 'ai' }
defineProps({
  message: {
    type: Object,
    required: true,
  },
});
</script>