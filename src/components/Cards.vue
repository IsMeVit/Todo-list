<script setup lang="ts">
const props = defineProps<{
  isOpen: boolean;
  stats: {
    lvl: number;
    hp: string;
    exp: number;
  };
  avatarUrl?: string; 
}>()

const emit = defineEmits(['close'])
</script>

<template>
  <Transition name="pixel-fade">
    <div v-if="isOpen" 
         class="fixed inset-0 z-[100] flex items-center justify-center bg-black/90 backdrop-blur-md p-4" 
         @click="$emit('close')">
      
      <div class="w-full max-w-sm bg-slate-950 border-4 border-red-600 shadow-[0_0_20px_rgba(220,38,38,0.5)] p-6 relative overflow-hidden" 
           @click.stop>
        
        <div class="absolute inset-0 pointer-events-none scanlines opacity-10"></div>

        <div class="flex flex-col items-center mb-6 border-b-4 border-double border-red-900/50 pb-4">
          <div class="w-24 h-24 bg-black border-4 border-red-600 overflow-hidden mb-3 shadow-[0_0_15px_rgba(220,38,38,0.3)]">
            <img v-if="avatarUrl" :src="avatarUrl" alt="Hero" class="w-full h-full object-cover boss-img" />
            <div v-else class="w-full h-full flex items-center justify-center text-4xl">💀</div>
          </div>
          
          <h2 class="text-xl font-black uppercase text-white leading-none scary-text">
            D4V15HH [SYSTEM OVERRIDE]
          </h2>
          <p class="text-[9px] text-red-500 font-bold uppercase tracking-[0.3em] mt-2 animate-pulse">
            Final Boss Developer
          </p>
        </div>

        <div class="space-y-3 font-bold uppercase text-xs">
          <div class="flex justify-between border-b-2 border-red-900/20 pb-1">
            <span class="text-slate-500 italic">Core_Level</span>
            <span class="text-white">{{ stats.lvl }}</span>
          </div>
          <div class="flex justify-between border-b-2 border-red-900/20 pb-1">
            <span class="text-slate-500 italic">Vulnerability</span>
            <span class="text-red-600 underline">NONE</span>
          </div>
          <div class="flex justify-between border-b-2 border-red-900/20 pb-1">
            <span class="text-slate-500 italic">Integrity</span>
            <span class="text-emerald-500">{{ stats.exp }} / ∞</span>
          </div>
        </div>

        <div class="mt-6 space-y-3">
          <p class="text-[8px] text-center text-red-900 font-bold uppercase tracking-widest">Execute Summoning Ritual</p>
          
          <div class="grid grid-cols-2 gap-3">
            <a href="mailto:ismevitt@gmail.com" target="_blank"
               class="summon-btn flex items-center justify-center gap-2 py-2 bg-black border-2 border-red-900 text-[10px] text-white font-bold transition-all hover:bg-red-950 hover:border-red-500">
               [ EMAIL ]
            </a>
            <a href="https://t.me/Ismevit_io" target="_blank"
               class="summon-btn flex items-center justify-center gap-2 py-2 bg-black border-2 border-red-900 text-[10px] text-white font-bold transition-all hover:bg-red-950 hover:border-red-500">
               [ Telegram ]
            </a>
          </div>
        </div>

        <button @click="$emit('close')" 
                class="w-full mt-6 py-2 bg-transparent text-red-600 border-2 border-red-600 text-xs font-black uppercase tracking-tighter hover:bg-red-600 hover:text-white transition-all">
          TERMINATE CONNECTION
        </button>

      </div>
    </div>
  </Transition>
</template>

<style scoped>

.scary-text {
  text-shadow: 2px 2px 0px #450a0a, -1px -1px 0px #ef4444;
  animation: glitch-shake 0.4s infinite;
}

@keyframes glitch-shake {
  0% { transform: translate(0); }
  25% { transform: translate(1px, -1px); }
  50% { transform: translate(-1px, 1px); }
  75% { transform: translate(1px, 1px); }
  100% { transform: translate(0); }
}

/* Image Filtering for "Cursed" look */
.boss-img {
  image-rendering: pixelated;
  filter: grayscale(0.8) sepia(0.3) brightness(0.7) contrast(1.2);
  transition: filter 0.3s;
}

.boss-img:hover {
  filter: grayscale(0) sepia(0) brightness(1);
}

/* CRT Scanlines CSS */
.scanlines {
  background: linear-gradient(
    rgba(18, 16, 16, 0) 50%, 
    rgba(0, 0, 0, 0.25) 50%
  );
  background-size: 100% 4px;
}

.summon-btn {
  box-shadow: 2px 2px 0px 0px #450a0a;
}

.summon-btn:active {
  box-shadow: none;
  transform: translate(2px, 2px);
}

/* Transition Animations */
.pixel-fade-enter-active, .pixel-fade-leave-active {
  transition: all 0.2s steps(4);
}
.pixel-fade-enter-from, .pixel-fade-leave-to {
  opacity: 0;
  transform: scale(0.9) translateY(10px);
}
</style>