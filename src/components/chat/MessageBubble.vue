<template>
  <div :class="['message-bubble', message.sender]">
    {{ message.text }}
  </div>
</template>

<script setup>
defineProps({
  message: {
    type: Object,
    required: true,
  },
});
</script>