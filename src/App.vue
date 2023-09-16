<script setup>
import { ref, onMounted, computed, watch } from 'vue'

// Handiling states with reference

const tasks = ref([])
const input_content = ref('')
const input_filter = ref('all')

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

const addTask = () => {
  if (input_content.value.trim() === '') {
    return
  }

  const newTask = {
    content: input_content.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  }
  tasks.value.push(newTask)
}

// Filter tasks by all, done or undone

const filteredTaks = computed(() =>
  tasks.value.filter((task) => {
    if (input_filter.value == 'all') {
      return true
    } else if (input_filter.value == 'done') {
      return task.done
    } else {
      return !task.done
    }
    return true
  })
)

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
      <div>
        <h3 class="text-xl text-bold text-orange-600 p-2">
          Create a new task !
        </h3>

        <!-- create new task form -->

        <form id="new-task-form" @submit.prevent="addTask">
          <div class="flex mt-4">
            <input
              type="text"
              name="content"
              id="content"
              placeholder="e.g. arrange for an online meeting"
              v-model="input_content"
              class="
                shadow
                appearance-none
                border
                rounded
                w-full
                py-2
                px-3
                mr-4
                text-grey-darker
              "
            />
            <input
              type="submit"
              value="Add task"
              class="
                flex-no-shrink
                p-2
                border-2
                rounded
                text-teal
                border-slate-400
                hover:text-white hover:bg-blue-400
              "
            />
          </div>
        </form>
      </div>

      <hr class="line" />

      <!-- Task List with filter -->

      <div>
        <div class="mt-5">
          <div class="flex justify-between">
            <h3 class="text-2xl text-bold">My Tasks List</h3>
            <div class="flex justify-between">
              <!-- Filter section -->

              <h1 class="text-bold">Filter by:</h1>
              <div class="mx-2">
                <div class="flex justify-between gap-2">
                  <div class="bubble_all">
                    <label class="flex gap-1 flex-col items-center">
                      <input
                        type="radio"
                        name="filter"
                        id="filter1"
                        value="all"
                        v-model="input_filter"
                      />
                      <div>All</div>
                    </label>
                  </div>

                  <div class="bubble_done">
                    <label class="flex gap-1 flex-col items-center">
                      <input
                        type="radio"
                        name="filter"
                        id="filter2"
                        value="done"
                        v-model="input_filter"
                      />

                      <div>Done</div>
                    </label>
                  </div>

                  <div class="bubble_undone">
                    <label class="flex gap-1 flex-col items-center">
                      <input
                        type="radio"
                        name="filter"
                        id="filter3"
                        value="notdone"
                        v-model="input_filter"
                      />

                      <div>undone</div>
                    </label>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Task List -->

          <div class="py-2" id="task-list">
            <div v-for="(task, index) in tasks" :key="index">
              <div
                :class="`flex items-center m-2 gap-3 ${
                  task.done && 'line-through'
                }`"
                v-if="
                  input_filter == 'all' ||
                  (input_filter == 'done' && task.done) ||
                  (input_filter == 'notdone' && !task.done)
                "
              >
                <input
                  type="checkbox"
                  class="checkbox_style"
                  v-model="task.done"
                />

                <div class="text-2xl grow">
                  <input
                    type="text"
                    :class="`
                   task
                    ${task.done && 'line-through    text-slate-400'}`"
                    v-model="task.content"
                  />
                </div>

                <div class="actions">
                  <button class="delete-button" @click="removeTask(index)">
                    Delete
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
