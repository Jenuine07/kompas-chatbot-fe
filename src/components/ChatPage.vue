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
import axios from 'axios'; 
import Sidebar from './layout/Sidebar.vue';
import Header from './layout/Header.vue';
import HeaderPrompt from './layout/HeaderPrompt.vue';
import ChatWindow from './chat/ChatWindow.vue';
import ChatInput from './chat/ChatInput.vue';
import '@/assets/styles/ChatPage.css';

// Data mock 
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
const currentChatId = ref(null); 
const isLoading = ref(false); 

const recentChats = computed(() => {
  return Object.keys(chatHistory).map(id => ({
    id: id,
    title: chatHistory[id].title,
  })).reverse();
});

// --- Functions ---
function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value;
}

function handleNewChat() {
  // Logic to save the current chat
  if (messages.value.length > 0 && currentChatId.value) {
    chatHistory[currentChatId.value] = {
      title: chatHistory[currentChatId.value]?.title || "Percakapan Baru",
      messages: [...messages.value]
    };
  } else if (messages.value.length > 0 && !currentChatId.value) {
     const newChatId = `chat-${Date.now()}`;
     const firstUserMessage = messages.value.find(m => m.sender === 'user');
     let newTitle = "Percakapan Baru";
     if (firstUserMessage) {
       newTitle = firstUserMessage.text.substring(0, 30) + (firstUserMessage.text.length > 30 ? '...' : '');
     }
     chatHistory[newChatId] = {
       title: newTitle,
       messages: [...messages.value]
     };
  }

  messages.value = [];
  currentChatId.value = null;
}

function handleLoadChat(chatId) {
  if (chatHistory[chatId]) {
    messages.value = [...chatHistory[chatId].messages];
    currentChatId.value = chatId; 
    if (!isSidebarOpen.value) {
      isSidebarOpen.value = true;
    }
  }
}

async function handleNewMessage(text) {
  messages.value.push({ id: Date.now(), text: text, sender: 'user' });

  isLoading.value = true; 
  try {
    const response = await axios.post('http://localhost:3000/api/chat', {
      prompt: text,
      sessionId: currentChatId.value || 'new-session'
    });

    // Add the bot's response from the backend
    messages.value.push({
      id: Date.now() + 1,
      text: response.data.response, 
      sender: 'ai'
    });

  } catch (error) {
    console.error("Error calling backend:", error);
    // Show an error message to the user
    messages.value.push({
      id: Date.now() + 1,
      text: "Sorry, I couldn't connect to the server.",
      sender: 'ai'
    });
  } finally {
    isLoading.value = false; 
  }
}

</script>