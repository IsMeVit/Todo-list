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
         class="fixed inset-0 z-[100] flex items-center justify-center bg-black/80 backdrop-blur-sm p-4" 
         @click="$emit('close')">
      
      <div class="w-full max-w-sm bg-white dark:bg-slate-900 border-4 border-black dark:border-white shadow-[8px_8px_0px_0px_rgba(0,0,0,1)] p-6 relative" 
           @click.stop>
        
        <div class="flex flex-col items-center mb-6 border-b-4 border-double border-black/10 dark:border-white/10 pb-4">
          <div class="w-24 h-24 bg-slate-200 dark:bg-slate-800 border-4 border-black dark:border-white overflow-hidden mb-3 shadow-[4px_4px_0px_0px_rgba(0,0,0,0.1)]">
            <img v-if="avatarUrl" :src="avatarUrl" alt="Hero" class="w-full h-full object-cover pixelated" />
            <div v-else class="w-full h-full flex items-center justify-center text-4xl">👤</div>
          </div>
          <h2 class="text-xl font-black uppercase dark:text-white leading-none">Super Hero Davishh</h2>
          <p class="text-[10px] text-blue-500 font-bold uppercase tracking-widest mt-1">Legendary Developer</p>
        </div>

        <div class="space-y-3 font-bold uppercase text-xs">
          <div class="flex justify-between border-b-2 border-black/5 dark:border-white/5 pb-1">
            <span class="text-slate-500">Level</span>
            <span class="text-yellow-500">{{ stats.lvl }}</span>
          </div>
          <div class="flex justify-between border-b-2 border-black/5 dark:border-white/5 pb-1">
            <span class="text-slate-500">Health</span>
            <span class="text-red-500">{{ stats.hp }}</span>
          </div>
          <div class="flex justify-between border-b-2 border-black/5 dark:border-white/5 pb-1">
            <span class="text-slate-500">Current XP</span>
            <span class="text-emerald-500">{{ stats.exp }} / 1000</span>
          </div>
        </div>

        <button @click="$emit('close')" 
                class="w-full mt-6 py-2 bg-red-600 text-white font-bold border-2 border-black shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] active:translate-x-[2px] active:translate-y-[2px] active:shadow-none transition-all">
          CLOSE STATS
        </button>

      </div>
    </div>
  </Transition>
</template>

<style scoped>
.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

.pixel-fade-enter-active, .pixel-fade-leave-active {
  transition: opacity 0.2s steps(4);
}
.pixel-fade-enter-from, .pixel-fade-leave-to {
  opacity: 0;
}
</style>