<template>
  <header class="main-header">
    <div class="header-title">
      Kompas AI <span class="project-divider">/</span> {{ projectTitle }}
    </div>
    <img :src="profilePicture" alt="Foto Profil" class="profile-icon">
  </header>
</template>

<script setup>
import profilePicture from '@/assets/profile.png';

// Terima 'projectTitle' sebagai prop dari ChatPage.vue
defineProps({
  projectTitle: {
    type: String,
    default: 'General'
  }
});
</script>