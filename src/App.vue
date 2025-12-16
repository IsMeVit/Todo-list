<script setup lang="ts">
import { ref } from 'vue'
import Navbar from './components/Navbar.vue'
import Home from './components/Home.vue'
import About from './components/about.vue'

// Logic: Tracks which view to show
const currentView = ref<'home' | 'about'>('home')

const handleNavigation = (page: 'home' | 'about') => {
  currentView.value = page
}
</script>

<template>
  <div class="h-screen flex flex-col overflow-hidden bg-gray-50">
    
    <Navbar @navigate="handleNavigation" />

    <div class="flex-1 overflow-y-auto">
      <Home v-if="currentView === 'home'" />
      <About v-if="currentView === 'about'" />
    </div>

  </div>
</template>

<style>
/* Global Pixel Font - ensure this is in your main CSS or App.vue */
@import url('https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400..700&display=swap');

.pixelify-sans {
  font-family: 'Pixelify Sans', sans-serif;
}
</style>