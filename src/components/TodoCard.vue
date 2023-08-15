<script setup>
import IconTrash from "@/components/icons/IconTrash.vue";

const emit = defineEmits(["remove"]);

defineProps({
  todo: {
    type: Object,
    required: true,
  },
});
</script>

<template>
  <div class="border border-gray-300 rounded p-4 block w-full mb-2">
    <div class="flex justify-between items-center">
      <div class="flex items-center">
        <input type="checkbox" v-model="todo.completed" />
        <span class="ml-2" :class="{ 'line-through': todo.completed }">
          {{ todo.text }}
        </span>
      </div>
      <button
        @click="emit('remove', todo)"
        type="button"
        class="rounded-full h-10 w-10 hover:bg-gray-300 dark-hover:bg-gray-700 transition-colors duration-200"
      >
        <div class="flex h-full w-full items-center justify-center">
          <IconTrash class="w-5 h-5 text-red-600 dark:text-red-600" />
        </div>
      </button>
    </div>
  </div>
</template>
