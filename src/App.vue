<script setup lang="ts">
import { ref, onMounted } from 'vue'
import Navbar from './components/Navbar.vue'
import Home from './components/Home.vue'
import About from './components/about.vue'

const isDark = ref(false)
const currentView = ref<'home' | 'about'>('home')

// Toggle function that also saves to your browser
const toggleTheme = () => {
  isDark.value = !isDark.value
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
}

// Check saved theme when the app starts
onMounted(() => {
  const saved = localStorage.getItem('theme')
  isDark.value = saved === 'dark'
})

const handleNavigation = (page: 'home' | 'about') => {
  currentView.value = page
}
</script>

<template>
  <div :data-theme="isDark ? 'dark' : 'light'" class="app-wrapper h-screen flex flex-col overflow-hidden pixelify-sans">
    
    <Navbar 
      :is-dark="isDark" 
      @toggle-theme="toggleTheme"
      @navigate="handleNavigation" 
    />

    <main class="flex-1 overflow-y-auto">
      <Home v-if="currentView === 'home'" />
      <About v-if="currentView === 'about'" />
    </main>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400..700&display=swap');

/* Default colors (Light Mode) */
.app-wrapper {
  --bg-main: #f9fafb;
  --text-main: #000000;
  background-color: var(--bg-main);
  color: var(--text-main);
  transition: all 0.3s ease;
}

/* Dark colors (Triggered by the data-theme attribute) */
[data-theme="dark"] {
  --bg-main: #0f172a;
  --text-main: #ffffff;
}

.pixelify-sans {
  font-family: 'Pixelify Sans', sans-serif;
}

/* Force children to respect the background */
main, .app-wrapper {
  background-color: var(--bg-main) !important;
}
</style>