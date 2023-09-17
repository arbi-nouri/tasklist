<script setup>
import { ref } from 'vue'
const input_filter = ref('all')

const props = defineProps(['tasks'])
</script>

<template>
  <div>
    <div class="mt-5">
      <div class="flex flex-col justify-between">
        <div class="flex flex-col justify-between">
          <!-- Filter section -->

          <h1 class="text-bold py-2">Filtered by:</h1>
          <div class="mx-2">
            <div class="flex justify-end gap-2">
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
            <!-- End of filter section -->
          </div>
        </div>
      </div>
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
            <input type="checkbox" class="checkbox_style" v-model="task.done" />

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
              <button class="delete-button" @click="$emit('removeTask', index)">
                Delete
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
