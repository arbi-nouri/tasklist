<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import TaskList from './components/TaskList.vue'
import InputTask from './components/TaskInput.vue'
import Alert from './components/Alert.vue'

// Handiling states with reference

const tasks = ref([])

// Watch any changes or updates in local storage

watch(
  tasks,
  (newVal) => {
    localStorage.setItem('tasks', JSON.stringify(newVal))
  },
  {
    deep: true,
  }
)

// Add a new task

const addTask = (input_content) => {
  if (input_content.trim() === '') {
    return
  }

  const newTask = {
    content: input_content,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  }
  tasks.value.push(newTask)
}

// Delete task from local storage

const removeTask = function (index) {
  tasks.value.splice(index, 1)
}

// Get local storage out from local storage

onMounted(() => {
  tasks.value = JSON.parse(localStorage.getItem('tasks')) || []
})
</script>

<template>
  <div class="card">
    <div class="w-full lg:w-1/2 p-4 lg:p-12">
      <h2 class="text-4xl font-bold">Greetings,</h2>

      <InputTask @addTask="addTask" />
      <hr class="line" />
      <h3 class="text-2xl text-bold">My Tasks List</h3>
      <div v-if="!tasks.length">
        <Alert />
      </div>
      <div v-if="tasks.length">
        <TaskList :tasks="tasks" @removeTask="removeTask" />
      </div>
    </div>
  </div>
</template>
