<script setup>
import { ref, computed } from 'vue'

// states: buat menyimpan data tugas atau tasks
const tasks = ref([])

// states: buat menyimpan inputan dari form
const inputTask = ref('')

// action: buat menambahkan tugas baru ke dalam daftar tasks
const addTask = (title) => {
  if (!title.trim()) return

  // bikin object task baru
  const newTask = {
    id: new Date().getTime(),
    title: title,
    status: false
  }

  tasks.value.push(newTask)
  inputTask.value = ''
}

// action: buat menghapus tugas
const deleteTask = (id) => {
  const task = tasks.value.find(item => item.id === id)

  if (!task) {
    alert("Task tidak ditemukan")
    return
  }

  // minta konfirmasi ke user
  const confirmation = confirm("Apakah anda yakin ingin menghapus?")
  
  if (confirmation) {
    // hapus task dari state tasks
    tasks.value = tasks.value.filter(item => item.id !== id)
  }
}

// helper computed untuk statistik bento card (opsional visual)
const completedCount = computed(() => tasks.value.filter(t => t.status).length)
const pendingCount = computed(() => tasks.value.length - completedCount.value)
</script>

<template>
  <main class="min-h-screen bg-pink-50/50 p-4 sm:p-8 md:p-12 font-sans text-gray-800">
    <div class="mx-auto max-w-5xl">
      
      <!-- Bento Grid Layout Container -->
      <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-4 auto-rows-[minmax(120px,auto)]">
        
        <!-- Bento Card 1: Header & App Branding (Pink Theme) -->
        <div class="md:col-span-2 lg:col-span-3 rounded-3xl bg-gradient-to-r from-pink-400 via-rose-400 to-pink-500 p-6 sm:p-8 text-white shadow-xl shadow-pink-200/50 flex flex-col justify-between relative overflow-hidden">
          <div class="absolute -right-6 -bottom-6 w-32 h-32 bg-white/10 rounded-full blur-xl pointer-events-none"></div>
          <div>
            <span class="inline-block px-3 py-1 bg-white/20 backdrop-blur-md rounded-full text-xs font-semibold tracking-wider uppercase mb-2">
              ✨ Productivity Hub
            </span>
            <h1 class="text-3xl sm:text-4xl font-black tracking-tight">Taskify Vue</h1>
            <p class="text-pink-100 mt-1 text-sm sm:text-base font-medium">Kelola tugas harianmu dengan tampilan Bento Grid yang berwarna!</p>
          </div>
        </div>

        <!-- Bento Card 2: Stats Ringkasan Total Task (Sky Blue Accent) -->
        <div class="rounded-3xl bg-gradient-to-br from-sky-100 to-blue-200 p-6 shadow-lg shadow-sky-100/50 flex flex-col justify-between border border-sky-200/60">
          <div class="flex justify-between items-center text-sky-700">
            <span class="text-xs font-bold uppercase tracking-wider">Total Task</span>
            <span class="text-xl">📋</span>
          </div>
          <div class="mt-4">
            <span class="text-4xl font-extrabold text-sky-900">{{ tasks.length }}</span>
            <p class="text-xs text-sky-600 font-medium mt-1">Item dalam daftar</p>
          </div>
        </div>

        <!-- Bento Card 3: Form Input Task Baru (Rose & Lavender Accent) -->
        <div class="md:col-span-3 lg:col-span-3 rounded-3xl bg-white p-6 shadow-xl shadow-pink-100/60 border border-pink-100 flex flex-col justify-center">
          <h2 class="text-lg font-bold text-gray-800 mb-3 flex items-center gap-2">
            <span>✨</span> Tambah Tugas Baru
          </h2>
          <form @submit.prevent="addTask(inputTask)" class="flex flex-col sm:flex-row gap-3">
            <input
              v-model="inputTask"
              type="text"
              placeholder="Apa yang ingin kamu kerjakan hari ini?"
              aria-label="Tugas baru"
              class="min-w-0 flex-1 rounded-2xl border border-pink-200 bg-pink-50/30 px-5 py-3.5 text-gray-700 placeholder-pink-300 focus:border-pink-500 focus:bg-white focus:outline-none focus:ring-4 focus:ring-pink-100 transition-all"
            />
            <button 
              type="submit"
              class="rounded-2xl bg-gradient-to-r from-pink-500 to-rose-500 px-7 py-3.5 font-bold text-white shadow-lg shadow-pink-200 hover:from-pink-600 hover:to-rose-600 active:scale-95 transition-all duration-200"
            >
              + Tambah
            </button>
          </form>
        </div>

        <!-- Bento Card 4: Quick Progress Badge (Emerald/Mint & Amber Accent) -->
        <div class="rounded-3xl bg-gradient-to-br from-emerald-100 to-teal-200 p-6 shadow-lg shadow-emerald-100/50 border border-emerald-200/60 flex flex-col justify-between">
          <div class="flex justify-between items-center text-emerald-800">
            <span class="text-xs font-bold uppercase tracking-wider">Status</span>
            <span class="text-xl">📊</span>
          </div>
          <div class="space-y-1">
            <div class="flex justify-between items-center text-sm">
              <span class="text-emerald-700 font-medium">Selesai:</span>
              <span class="font-bold text-emerald-900">{{ completedCount }}</span>
            </div>
            <div class="flex justify-between items-center text-sm">
              <span class="text-amber-700 font-medium">Pending:</span>
              <span class="font-bold text-amber-900">{{ pendingCount }}</span>
            </div>
          </div>
        </div>

        <!-- Bento Card 5: Daftar Tugas Main Grid (Violet / Pink Soft Theme) -->
        <div class="md:col-span-3 lg:col-span-4 rounded-3xl bg-white p-6 sm:p-8 shadow-xl shadow-pink-100/60 border border-pink-100">
          <div class="flex items-center justify-between mb-6">
            <h2 class="text-xl font-bold text-gray-800 flex items-center gap-2">
              <span>📌</span> Daftar Tugas
            </h2>
            <span class="text-xs font-semibold px-3 py-1 rounded-full bg-violet-100 text-violet-700">
              {{ tasks.length }} Tugas
            </span>
          </div>

          <!-- Empty State jika belum ada tugas -->
          <div v-if="tasks.length === 0" class="text-center py-12 bg-pink-50/40 rounded-2xl border-2 border-dashed border-pink-200">
            <p class="text-4xl mb-2">🎈</p>
            <p class="text-gray-500 font-medium">Belum ada tugas. Tambahkan tugas pertamamu di atas!</p>
          </div>

          <!-- List Task -->
          <ul v-else class="space-y-3" v-auto-animate>
            <li 
              v-for="task in tasks" 
              :key="task.id"
              class="group flex items-center justify-between rounded-2xl p-4 transition-all duration-200 border"
              :class="task.status 
                ? 'bg-gray-50 border-gray-200 opacity-70' 
                : 'bg-gradient-to-r from-pink-50/60 to-purple-50/60 border-pink-100 hover:border-pink-300 hover:shadow-md hover:shadow-pink-100/50'"
            >
              <div class="flex items-center gap-3.5 min-w-0 flex-1">
                <input 
                  v-model="task.status"
                  type="checkbox" 
                  class="h-5 w-5 rounded-lg border-pink-300 text-pink-500 focus:ring-pink-400 cursor-pointer accent-pink-500" 
                />
                <span 
                  class="text-base font-medium transition-all duration-200 truncate pr-2" 
                  :class="task.status ? 'line-through text-gray-400' : 'text-gray-700'"
                >
                  {{ task.title }}
                </span>
              </div>

              <button 
                @click="deleteTask(task.id)"
                class="rounded-xl bg-rose-50 px-3.5 py-2 text-xs font-semibold text-rose-500 hover:bg-rose-500 hover:text-white transition-all duration-200 active:scale-95 flex items-center gap-1"
              >
                <span>🗑️</span> Hapus
              </button>
            </li>
          </ul>
        </div>

      </div>
    </div>
  </main>
</template>