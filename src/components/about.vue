<script setup>
import { ref, onMounted } from 'vue';
import Cards from './Cards.vue'

const userName = ref('Guest');
const isCardOpen = ref(false); 
const myAvatar = ref('https://i.pinimg.com/736x/41/6a/cd/416acd7cd5c3c454b610b3980f5c89f5.jpg');

onMounted(() => {
  const savedName = localStorage.getItem('pixel-username');
  if (savedName) userName.value = savedName;
});

const resetGame = () => {
  if (confirm("Are you sure you want to delete your save data? All quests and your name will be lost!")) {
    localStorage.clear();
    window.location.reload();
  }
};

const myStats = ref({
  lvl: '999999',
  hp: 'Unkillable',
  exp:'Unlimited'
})
</script>

<template>
  <main id="about" class="min-h-screen bg-gray-50 pixelify-sans p-4 md:p-8">
    <div class="max-w-2xl mx-auto">
      
      <header class="text-center mb-12">
        <h2 class="text-4xl md:text-5xl font-bold uppercase tracking-widest border-b-8 border-black inline-block pb-2 ">
          Instruction Manual
        </h2>
      </header>

      <section class="bg-white border-4 border-black p-6 shadow-[8px_8px_0px_0px_rgba(0,0,0,1)] mb-8">
        <h3 class="text-2xl font-bold text-blue-600 mb-4 uppercase tracking-tighter underline">The Objective</h3>
        <p class="text-lg text-gray-700 leading-relaxed">
          Welcome to <span class="font-bold">Todo List RPG</span>. This isn't just a productivity tool; it's a quest log for your daily life. 
          Complete your tasks to gain focus, clear your mind, and level up your real-world efficiency.
        </p>
      </section>

      <section class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
        <div class="bg-blue-100 border-4 border-black p-4 shadow-[4px_4px_0px_0px_rgba(0,0,0,1)]">
          <h4 class="ffont-bold uppercase mb-2">Current Hero</h4>
          <p class="text-2xl font-bold text-blue-800">{{ userName }}</p>
        </div>
        <div class="bg-yellow-100 border-4 border-black p-4 shadow-[4px_4px_0px_0px_rgba(0,0,0,1)]">
          <h4 class="font-bold uppercase mb-2 ">Version</h4>
          <p class="text-2xl font-bold text-yellow-800 ">v1.0.1-Pixel</p>
        </div>
      </section>

      <section class="bg-red-50 border-4 border-red-600 p-6 shadow-[8px_8px_0px_0px_rgba(220,38,38,1)]">
        <h3 class="text-2xl font-bold text-red-600 mb-2 uppercase tracking-tighter">Danger Zone</h3>
        <p class="text-gray-600 mb-4 italic">Warning: This will permanently delete your character and all saved quests.</p>
        
        <button 
          @click="resetGame"
          class="bg-red-600 text-white px-6 py-3 border-4 border-black font-bold uppercase shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] hover:bg-red-700 hover:-translate-y-1 active:translate-y-1 active:shadow-none transition-all"
        >
          Reset Save Data
        </button>
      </section>

      <footer class="mt-10 text-center uppercase text-slate-400 text-xs tracking-widest">
        DEVELOPED BY 
        <span 
          @click="isCardOpen = true" 
          class="text-blue-500 underline cursor-pointer hover:text-blue-400 transition-colors font-bold"
        >
          D4V15HH
        </span> 
        © 2025. ALL RIGHTS RESERVED
      </footer>

      <Cards 
        :is-open="isCardOpen" 
        :stats="myStats"
        :avatar-url="myAvatar"
        @close="isCardOpen = false" 
      />

    </div>
  </main>
</template>