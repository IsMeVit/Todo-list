<script setup lang="ts">
import { ref } from 'vue'

// Define the custom events this component can send
const emit = defineEmits<{
  (e: 'navigate', page: 'home' | 'about'): void
}>()

const isOpen = ref(false)

const goTo = (page: 'home' | 'about') => {
  emit('navigate', page)
  isOpen.value = false // Close mobile menu after clicking
}
</script>

<template>
  <nav class="navbar pixelify-sans sticky top-0 z-50 bg-white border-b-4 border-black shadow-[4px_4px_0px_0px_rgba(0,0,0,1)]">
    <div class="flex items-center justify-between px-6 py-4 max-w-7xl mx-auto">
      
      <h1 @click="goTo('home')" class="title text-2xl md:text-3xl font-bold tracking-wider hover:text-blue-600 cursor-pointer transition-colors">
        Todo List
      </h1>

      <button 
        @click="isOpen = !isOpen" 
        class="md:hidden flex flex-col gap-1 p-2 border-2 border-black shadow-[2px_2px_0px_0px_rgba(0,0,0,1)] bg-white active:translate-x-[2px] active:translate-y-[2px] active:shadow-none"
      >
        <div class="w-6 h-1 bg-black"></div>
        <div class="w-6 h-1 bg-black"></div>
        <div class="w-6 h-1 bg-black"></div>
      </button>

      <div class="hidden md:flex gap-8 items-center text-xl">
        <button @click="goTo('home')" class="nav-item">Home</button>
        <button @click="goTo('about')" class="nav-item">About</button>
        
        <a href="https://github.com" target="_blank"
           class="bg-yellow-300 px-4 py-1 border-2 border-black text-sm font-bold shadow-[2px_2px_0px_0px_rgba(0,0,0,1)] transition-all hover:bg-yellow-400 hover:-translate-y-0.5 active:translate-y-0 active:shadow-none">
          Github
        </a>
      </div>
    </div>

    <div v-if="isOpen" class="md:hidden border-t-4 border-black bg-white flex flex-col items-center gap-6 py-8">
      <button @click="goTo('home')" class="text-xl font-bold hover:text-blue-600">Home</button>
      <button @click="goTo('about')" class="text-xl font-bold hover:text-blue-600">About</button>
      <a href="https://github.com/" target="_blank" 
         class="bg-yellow-300 px-6 py-2 border-2 border-black text-lg font-bold shadow-[2px_2px_0px_0px_rgba(0,0,0,1)]">
        Github
      </a>
    </div>
  </nav>
</template>

<style scoped>
.nav-item {
  position: relative;
  transition: all 0.2s ease;
  font-weight: bold;
}
.nav-item:hover {
  transform: translateY(-2px);
  color: #2563eb;
}
.nav-item::after {
  content: '';
  position: absolute;
  width: 0;
  height: 3px;
  bottom: -4px;
  left: 0;
  background-color: #2563eb;
  transition: width 0.3s;
}
.nav-item:hover::after {
  width: 100%;
}
</style>