<script setup>
import { ref, watch } from "vue";

const props = defineProps(["task"]);
const emit = defineEmits(["close", "save"]);

const editedTask = ref({});

watch(
  () => props.task,
  (newTask) => {
    editedTask.value = { ...newTask };
  },
  { immediate: true }
);

const saveTask = () => {
  emit("save", editedTask.value);
  emit("close");
};
</script>

<template>
  <div class="overlay">
    <div class="modal">
      <h2>Edit Task</h2>

      <input v-model="editedTask.title" placeholder="Task Title" />

      <textarea
        v-model="editedTask.description"
        placeholder="Task Description"
        rows="3"
      ></textarea>

      <select v-model="editedTask.priority">
        <option value="Low">Low Priority</option>
        <option value="Medium">Medium Priority</option>
        <option value="High">High Priority</option>
      </select>

      <select v-model="editedTask.category">
        <option value="Work">Work</option>
        <option value="Personal">Personal</option>
        <option value="Urgent">Urgent</option>
      </select>

      <div class="btn-group">
        <button class="save" @click="saveTask">Save</button>
        <button class="cancel" @click="$emit('close')">Cancel</button>
      </div>
    </div>
  </div>
</template>

<style>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: white;
  padding: 25px;
  width: 350px;
  border-radius: 12px;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  gap: 12px;
}

input,
textarea,
select {
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

.btn-group {
  display: flex;
  justify-content: space-between;
}

.save {
  background: #4e73df;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
}

.cancel {
  background: #858796;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
}
</style>