<script setup>
import TaskItem from "./TaskItem.vue";

defineProps(["tasks"]);
const emit = defineEmits(["delete-task", "update-task"]);
</script>

<template>
  <div>
    <TaskItem
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      @delete-task="emit('delete-task', $event)"
      @update-task="emit('update-task', $event)"
    />
  </div>
</template>