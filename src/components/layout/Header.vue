<template>
  <header class="main-header">
    <div class="header-title">
      Kompas AI
    </div>
    
    <img :src="profilePicture" alt="Foto Profil" class="profile-icon">

  </header>
</template>

<script setup>
// Impor gambar profil yang baru
import profilePicture from '@/assets/profile.png';
</script>