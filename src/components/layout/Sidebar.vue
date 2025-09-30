<template>
  <aside class="sidebar">
    <div class="sidebar-item logo" @click="emit('toggleSidebar')">
      <img v-if="!isOpen" :src="kompasLogo" alt="Kompas Logo" class="kompas-logo-collapsed">
      
      <template v-if="isOpen">
        <img :src="kompasLogo" alt="Kompas AI Logo" class="kompas-logo-full">
        <span class="text-and-arrow">
          <span class="text">Kompas AI</span>
          <span class="arrow-icon">◀</span>
        </span>
      </template>
    </div>
    
    <button class="sidebar-item new-chat-btn" @click="emit('startNewChat')">
      <span class="icon">✎</span>
      <span v-show="isOpen" class="text">New Chat</span>
    </button>
    
    <div v-show="isOpen" class="recent-title">Recent</div>
    
    <ul v-show="isOpen" class="recent-list">
      <li 
        class="sidebar-item"
        v-for="chat in chats" 
        :key="chat.id" 
        @click="emit('loadChat', chat.id)"
      >
        <span class="icon">📄</span>
        <span class="text">{{ chat.title }}</span>
      </li>
    </ul>

    <div class="sidebar-footer">
       <div class="sidebar-item settings">
         <span class="icon">⚙️</span>
         <span v-show="isOpen" class="text">Setelan & Bantuan</span>
       </div>
    </div>
  </aside>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';
import kompasLogo from '@/assets/kompas-logo.png';

defineProps({
  chats: { type: Array, required: true },
  isOpen: { type: Boolean, required: true },
});

const emit = defineEmits(['toggleSidebar', 'startNewChat', 'loadChat']);
</script>