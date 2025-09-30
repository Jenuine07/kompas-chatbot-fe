<template>
  <div :class="['chat-page-container', { 'sidebar-closed': !isSidebarOpen }]">
    <Sidebar 
      @toggleSidebar="toggleSidebar" 
      @startNewChat="handleNewChat"
      @loadChat="handleLoadChat"
      :chats="recentChats"
      :isOpen="isSidebarOpen" 
    />
    <main class="main-content">
      
      <Header />

      <HeaderPrompt v-if="messages.length === 0" @sendPrompt="handleNewMessage" />
      <ChatWindow v-else :messages="messages" />
      <ChatInput @sendMessage="handleNewMessage" />
    </main>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue';
import Sidebar from './layout/Sidebar.vue';
import Header from './layout/Header.vue'; // TAMBAHKAN IMPORT INI
import HeaderPrompt from './layout/HeaderPrompt.vue';
import ChatWindow from './chat/ChatWindow.vue';
import ChatInput from './chat/ChatInput.vue';
import '@/assets/styles/ChatPage.css';

// Data mock untuk history, Anda bisa biarkan seperti ini
const chatHistory = reactive({
  'sejarah-indonesia': {
    title: "Sejarah Indonesia Merdeka",
    messages: [
      { id: 101, text: 'Ceritakan tentang sejarah kemerdekaan Indonesia secara singkat.', sender: 'user' },
      { id: 102, text: 'Tentu. Perjuangan kemerdekaan Indonesia mencapai puncaknya dengan Proklamasi pada 17 Agustus 1945...', sender: 'ai' },
    ],
   },
  'vue-vs-react': {
    title: "Perbedaan Vue dan React",
    messages: [
      { id: 201, text: 'Apa saja perbedaan utama antara Vue.js dan React?', sender: 'user' },
      { id: 202, text: 'Perbedaan utama terletak pada pendekatan...', sender: 'ai' },
    ],
   },
  'tips-clean-code': {
    title: "Tips Belajar Clean Code",
    messages: [
      { id: 301, text: 'Berikan 3 tips utama untuk belajar clean code.', sender: 'user' },
      { id: 302, text: '1. Gunakan nama variabel yang jelas... 2. Buat fungsi yang kecil... 3. Konsisten...', sender: 'ai' },
    ],
   },
});

const isSidebarOpen = ref(true);
const messages = ref([]);

const recentChats = computed(() => {
  return Object.keys(chatHistory).map(id => ({
    id: id,
    title: chatHistory[id].title,
  })).reverse();
});

function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value;
}

function handleNewChat() {
  if (messages.value.length > 0) {
    const newChatId = `chat-${Date.now()}`;
    const firstUserMessage = messages.value.find(m => m.sender === 'user');
    let newTitle = "Percakapan Baru";
    if (firstUserMessage) {
      newTitle = firstUserMessage.text.substring(0, 30);
      if (firstUserMessage.text.length > 30) {
        newTitle += '...';
      }
    }
    chatHistory[newChatId] = {
      title: newTitle,
      messages: [...messages.value] 
    };
  }
  messages.value = [];
}

function handleLoadChat(chatId) {
  if (chatHistory[chatId]) {
    messages.value = [...chatHistory[chatId].messages];
    if (!isSidebarOpen.value) {
      isSidebarOpen.value = true;
    }
  }
}

function handleNewMessage(text) {
  messages.value.push({ id: Date.now(), text: text, sender: 'user' });
  simulateAiResponse(text);
}

function simulateAiResponse(userText) {
  setTimeout(() => {
    messages.value.push({ id: Date.now() + 1, text: `Ini adalah balasan otomatis untuk pesan Anda: "${userText}"`, sender: 'ai' });
  }, 1000);
}
</script>