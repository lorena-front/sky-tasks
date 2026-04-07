<script setup>
import { ref, watch, onMounted } from 'vue'
import './style.css'

const task = ref('')
const tasks = ref([])

onMounted(() => {
  const storedItems = localStorage.getItem('myContent')
  if (storedItems) {
    tasks.value = JSON.parse(storedItems)
  }
})

watch(
  tasks,
  (newList) => {
    localStorage.setItem('myContent', JSON.stringify(newList))
  },
  { deep: true },
)

const addTask = () => {
  if (task.value.trim() !== '') {
    tasks.value.push({
      text: task.value,
      editing: false,
    })
    task.value = ''
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}
</script>

<template>
  <main>
    <div class="card">
      <h1>Tasks <span class="material-symbols-outlined">ink_pen</span></h1>
      <div class="card-content">
        <div class="card-input">
          <input type="text" v-model="task" @keyup.enter="addTask" maxlength="80" />
          {{ task.length }}/80
          <button @click="addTask">Add</button>
        </div>
        <ul>
          <li v-for="(task, index) in tasks" :key="index">
            <span v-if="!task.editing">
              {{ task.text }}
            </span>

            <textarea maxlength="80" class="editInput" v-else v-model="task.text" @keyup.enter="task.editing = false" />

            <span class="material-symbols-outlined delete-btn" @click="task.editing = !task.editing">
              edit
            </span>

            <span class="material-symbols-outlined delete-btn" @click="deleteTask(index)">
              close
            </span>
          </li>
        </ul>
        <p class="empty-msg" v-if="tasks.length === 0">There's nothing yet.</p>
      </div>
    </div>
    <a href="https://github.com/lorena-front" target="_blank" rel="noopener noreferrer">
      <img class="heart-icon" src="./assets/images/pixel-heart.gif" alt="" />
    </a>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1 {
  font-size: 1.3rem;
  margin: 1rem 0 1rem 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-weight: 500;
}

.card {
  width: 20rem;
  height: 25rem;
  overflow-y: auto;
  padding: 0.9375rem;
  box-shadow: 8px 8px 0 black;
  background-color: rgb(255 182 214 / 92%);
  backdrop-filter: blur(2px);
  transform: rotate(-0.5deg);
}

.card-content {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  align-items: center;
}

.card-input {
  margin-top: 1rem;
  display: flex;
  align-items: center;
  gap: .2rem;
}

.card-input input,
.card-input button {
  padding: 0.4rem 0.5rem;
}

.card-input input {
  border: 2px solid black;
  border-bottom: 5px solid black;
  border-right: 5px solid black;
}

.card-input button {
  margin-left: 1rem;
  font-family: var(--inter-font);
  border: 2px solid black;
  border-bottom: 5px solid black;
  border-right: 5px solid black;
  cursor: pointer;
}

.card-input button:hover {
  transform: scale(1.05);
}

ul li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  line-height: 2;
  overflow-wrap: anywhere;
  max-width: 100%;
}

li span:first-child {
  flex: 1;
  min-width: 0;
}

.material-symbols-outlined {
  flex-shrink: 0;
}

ul li:hover {
  transform: translateX(3px);
}

.delete-btn {
  cursor: pointer;
  font-size: 1.3rem;
}

.empty-msg {
  opacity: 0.7;
  font-size: 0.95rem;
  font-style: italic;
}

.heart-icon {
  width: 2.8125rem;
  position: fixed;
  bottom: clamp(10%, 8vw, 15%);
  left: 51%;
  transform: translateX(-50%);
}

.editInput {
  width: 12.75rem;
  height: 5rem;
}
</style>
