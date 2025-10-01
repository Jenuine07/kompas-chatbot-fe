<template>
  <div class="header-prompt">
    <h1 class="greeting">
      <span class="greeting-static">Halo, </span>
      <span class="greeting-name">Kompas</span>
    </h1>
    <p class="subtitle">Bagaimana saya bisa membantu Anda hari ini?</p>
    
    <div class="prompt-buttons">
      <button @click="emitPrompt('Jelaskan tentang clean code')">
        Jelaskan tentang clean code
      </button>
      <button @click="emitPrompt('Buatkan contoh layout CSS Flexbox')">
        Buatkan contoh layout CSS Flexbox
      </button>
      <button @click="emitPrompt('Apa itu Vue Composition API?')">
        Apa itu Vue Composition API?
      </button>
    </div>
  </div>
</template>

<script setup>
// Fungsi emit tetap sama, tidak perlu diubah
const emit = defineEmits(['sendPrompt']);

function emitPrompt(promptText) {
  emit('sendPrompt', promptText);
}
</script>