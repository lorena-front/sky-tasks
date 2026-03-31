<script setup>
import { ref } from 'vue'
import './style.css'

const task = ref('')
const tasks = ref([])

const addTask = () => {
  if (task.value.trim() !== '') {
    tasks.value.push(task.value)
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
      <h1>Todo App</h1>
      <div class="card-content">
        <div class="card-input">
          <input type="text" v-model="task" @keyup.enter="addTask" />
          <button @click="addTask">Add</button>
        </div>
        <ul>
          <li v-for="(item, index) in tasks" :key="index">
            {{ item }}

            <span class="material-symbols-outlined" @click="deleteTask(index)">
              close
            </span>
          </li>
        </ul>
        <p v-if="tasks.length === 0">
          There's nothing yet.
        </p>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
}

.card {
  width: 20rem;
  height: 25rem;
  text-align: center;
}

.card-content {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  align-items: center;
}

.card-input {
  margin-top: 1rem;
}

.card-input input,
.card-input button {
  padding: 0.2rem 0.5rem;
}

.card-input button {
  margin-left: 1rem;
}

ul li {
  display: flex;
  align-items: center;
}

span {
  cursor: pointer;
}
</style>
