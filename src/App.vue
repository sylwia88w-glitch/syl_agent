<template>
  <div class="app">
    <h1>Lista Zadań</h1>

    <div class="input-section">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Dodaj nowe zadanie..."
        class="task-input"
      />
      <button @click="addTask" class="add-btn">Dodaj</button>
    </div>

    <div class="filters">
      <button
        v-for="filter in filters"
        :key="filter.value"
        @click="activeFilter = filter.value"
        :class="['filter-btn', { active: activeFilter === filter.value }]"
      >
        {{ filter.label }}
      </button>
    </div>

    <ul class="task-list">
      <li
        v-for="task in filteredTasks"
        :key="task.id"
        :class="['task-item', { done: task.done }]"
      >
        <input type="checkbox" v-model="task.done" class="checkbox" />
        <span class="task-text">{{ task.text }}</span>
        <button @click="removeTask(task.id)" class="remove-btn">✕</button>
      </li>
    </ul>

    <p v-if="tasks.length === 0" class="empty">Brak zadań. Dodaj pierwsze!</p>

    <div v-if="tasks.length > 0" class="summary">
      {{ doneTasks }} / {{ tasks.length }} zadań ukończonych
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const activeFilter = ref('all')
const tasks = ref([
  { id: 1, text: 'Nauczyć się Vue.js', done: false },
  { id: 2, text: 'Zbudować pierwszą aplikację', done: true },
])

const filters = [
  { value: 'all', label: 'Wszystkie' },
  { value: 'active', label: 'Do zrobienia' },
  { value: 'done', label: 'Ukończone' },
]

const filteredTasks = computed(() => {
  if (activeFilter.value === 'active') return tasks.value.filter(t => !t.done)
  if (activeFilter.value === 'done') return tasks.value.filter(t => t.done)
  return tasks.value
})

const doneTasks = computed(() => tasks.value.filter(t => t.done).length)

function addTask() {
  const text = newTask.value.trim()
  if (!text) return
  tasks.value.push({ id: Date.now(), text, done: false })
  newTask.value = ''
}

function removeTask(id) {
  tasks.value = tasks.value.filter(t => t.id !== id)
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Segoe UI', sans-serif;
  background: #f0f2f5;
  display: flex;
  justify-content: center;
  padding: 40px 16px;
  min-height: 100vh;
}

.app {
  background: white;
  border-radius: 12px;
  padding: 32px;
  width: 100%;
  max-width: 480px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  height: fit-content;
}

h1 {
  font-size: 1.8rem;
  color: #1a1a2e;
  margin-bottom: 24px;
  text-align: center;
}

.input-section {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

.task-input {
  flex: 1;
  padding: 10px 14px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.2s;
}

.task-input:focus {
  border-color: #4f46e5;
}

.add-btn {
  padding: 10px 18px;
  background: #4f46e5;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}

.add-btn:hover {
  background: #4338ca;
}

.filters {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

.filter-btn {
  flex: 1;
  padding: 6px;
  border: 2px solid #e0e0e0;
  border-radius: 6px;
  background: white;
  cursor: pointer;
  font-size: 0.85rem;
  transition: all 0.2s;
}

.filter-btn.active {
  border-color: #4f46e5;
  background: #4f46e5;
  color: white;
}

.task-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.task-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px;
  background: #f8f9fa;
  border-radius: 8px;
  transition: opacity 0.2s;
}

.task-item.done {
  opacity: 0.5;
}

.task-item.done .task-text {
  text-decoration: line-through;
}

.checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #4f46e5;
}

.task-text {
  flex: 1;
  font-size: 1rem;
  color: #333;
}

.remove-btn {
  background: none;
  border: none;
  color: #999;
  cursor: pointer;
  font-size: 1rem;
  padding: 2px 6px;
  border-radius: 4px;
  transition: color 0.2s, background 0.2s;
}

.remove-btn:hover {
  color: #e53e3e;
  background: #fff0f0;
}

.empty {
  text-align: center;
  color: #999;
  padding: 24px 0;
}

.summary {
  margin-top: 16px;
  text-align: center;
  color: #666;
  font-size: 0.9rem;
}
</style>
