<script setup>
import { ref } from 'vue'
const input_content = ref('')
</script>

<template>
  <div>
    <h3 class="text-xl text-bold text-orange-600 p-2">Create a new task !</h3>
    <form
      id="new-task-form"
      @submit.prevent="() => $emit('addTask', input_content)"
    >
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
</template>
