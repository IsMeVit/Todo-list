<script setup lang="ts">
import { ref, onMounted, watch, computed } from 'vue';

// Define the shape of our Quest data
interface Quest {
  id: number;
  text: string;
  completed: boolean;
}

// --- STATE ---
const userName = ref<string>('');
const isEditingName = ref<boolean>(false);
const newTask = ref<string>('');
const tasks = ref<Quest[]>([]);

// --- PERSISTENCE ---
onMounted(() => {
  const savedTasks = localStorage.getItem('pixel-tasks');
  if (savedTasks) tasks.value = JSON.parse(savedTasks);
  
  const savedName = localStorage.getItem('pixel-username');
  if (savedName && savedName.trim() !== '') {
    userName.value = savedName;
    isEditingName.value = false;
  } else {
    isEditingName.value = true; 
  }
});

watch(tasks, (newVal: Quest[]) => {
  localStorage.setItem('pixel-tasks', JSON.stringify(newVal));
}, { deep: true });

// --- METHODS ---
const saveName = (): void => {
  if (userName.value.trim() !== '') {
    localStorage.setItem('pixel-username', userName.value);
    isEditingName.value = false;
  }
};

const addQuest = (): void => {
  if (!hasName.value || newTask.value.trim() === '') return;
  tasks.value.push({
    id: Date.now(),
    text: newTask.value,
    completed: false
  });
  newTask.value = '';
};

const deleteQuest = (id: number): void => {
  tasks.value = tasks.value.filter(q => q.id !== id);
};

// --- COMPUTED ---
const hasName = computed(() => userName.value.trim() !== '' && !isEditingName.value);
const tasksLeft = computed(() => tasks.value.filter(t => !t.completed).length);
</script>

<template>
  <main class="min-h-screen bg-gray-100 pixelify-sans p-4 md:p-10">
    <div class="max-w-2xl mx-auto">
      
      <header class="text-center mb-8 md:mb-12">
        <div v-if="isEditingName" class="w-full bg-yellow-100 border-4 border-black p-5 md:p-8 shadow-[6px_6px_0px_0px_rgba(0,0,0,1)] inline-block">
          <h2 class="text-xl md:text-3xl font-bold uppercase mb-4 tracking-tighter">New Game: Register Hero</h2>
          <div class="flex flex-col gap-4">
            <input 
              v-model="userName" 
              @keyup.enter="saveName"
              class="border-4 border-black p-3 text-lg outline-none shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] focus:shadow-none transition-all" 
              placeholder="Enter name..."
            />
            <button 
              @click="saveName" 
              class="bg-blue-600 text-white border-4 border-black px-8 py-3 font-bold text-lg shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] active:shadow-none active:translate-x-1 active:translate-y-1 transition-all"
            >
              START GAME
            </button>
          </div>
          <p v-if="userName.trim() === ''" class="mt-4 text-red-600 text-xs md:text-sm font-bold uppercase italic">
            * Name Required to unlock quests
          </p>
        </div>

        <div v-else @click="isEditingName = true" class=" group">
          <h2 class="text-3xl md:text-6xl font-bold break-words">
            Welcome, <span class="text-blue-600 cursor-pointer underline underline-offset-4 md:underline-offset-8">{{ userName }}</span>
          </h2>
          <p class="text-xs md:text-sm text-gray-400 mt-2 md:mt-4 uppercase">(Tap to rename character)</p>
        </div>
      </header>

      <section 
        class="bg-white border-4 border-black p-4 md:p-6 shadow-[6px_6px_0px_0px_rgba(0,0,0,1)] mb-8 md:mb-12 relative"
        :class="{'opacity-50 select-none': !hasName}"
      >
        <div v-if="!hasName" class="absolute inset-0 z-30 flex items-center justify-center bg-gray-200/60 backdrop-blur-[2px]">
           <div class="bg-black text-white px-3 py-1 border-2 border-white shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] font-bold text-xs md:text-base uppercase">
             Locked: Enter Name
           </div>
        </div>

        <form @submit.prevent="addQuest" class="flex flex-col md:flex-row gap-3 md:gap-4">
          <input 
            v-model="newTask"
            :disabled="!hasName"
            type="text" 
            placeholder="New Quest..." 
            class="flex-1 border-4 border-black p-3 text-lg focus:bg-blue-50 outline-none"
          />
          <button 
            type="submit"
            :disabled="!hasName"
            class="bg-green-400 px-6 py-3 border-4 border-black font-bold text-lg shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] active:shadow-none active:translate-y-1 transition-all disabled:opacity-50"
          >
            ADD +
          </button>
        </form>
      </section>

      <section 
        class="transition-all duration-500 pb-10"
        :class="{'blur-sm pointer-events-none grayscale': !hasName}"
      >
        <div class="flex justify-between items-end border-b-4 border-black mb-4 md:mb-6 px-1">
          <h3 class="text-xl md:text-2xl font-bold uppercase tracking-widest">Quest Log</h3>
          <span class="text-xs md:text-sm font-bold pb-1 text-gray-500">{{ tasksLeft }} Pending</span>
        </div>

        <div v-if="tasks.length === 0" class="text-center py-10 border-4 border-dashed border-gray-300 text-gray-400 uppercase italic text-sm">
          No quests active.
        </div>

        <div 
          v-for="task in tasks" 
          :key="task.id" 
          class="group bg-white border-4 border-black p-3 md:p-4 mb-3 flex items-center justify-between shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] active:translate-x-1 transition-all"
        >
          <div class="flex items-center gap-3 md:gap-4 flex-1">
            <input type="checkbox" v-model="task.completed" class="checkbox-pixel shrink-0" />
            <span :class="{'line-through text-gray-400 italic': task.completed}" class="text-base md:text-xl font-medium break-all">
              {{ task.text }}
            </span>
          </div>
          <button @click="deleteQuest(task.id)" class="text-red-600 font-bold hover:scale-125 px-2 transition-all text-sm md:text-base">
            [X]
          </button>
        </div>
      </section>

    </div>
  </main>
</template>

<style scoped>
.checkbox-pixel {
  appearance: none;
  background-color: #fff;
  margin: 0;
  width: 1.25rem;
  height: 1.25rem;
  border: 3px solid black;
  display: grid;
  place-content: center;
  cursor: pointer;
}

@media (min-width: 768px) {
  .checkbox-pixel {
    width: 1.5rem;
    height: 1.5rem;
    border: 4px solid black;
  }
}

.checkbox-pixel::before {
  content: "";
  width: 0.6rem;
  height: 0.6rem;
  transform: scale(0);
  transition: 100ms transform ease-in-out;
  background-color: black;
  box-shadow: 0 0 0 2px white;
}

.checkbox-pixel:checked::before {
  transform: scale(1);
}
</style>