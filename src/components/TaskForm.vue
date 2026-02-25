<script setup>
import { ref } from "vue";

const emit = defineEmits(["add-task"]);

const title = ref("");
const description = ref("");
const priority = ref("Low");
const category = ref("Work");

const submitTask = () => {
  if (!title.value.trim()) return;

  emit("add-task", {
    id: Date.now(),
    title: title.value,
    description: description.value,
    priority: priority.value,
    category: category.value,
    status: "Pending"
  });

  title.value = "";
  description.value = "";
  priority.value = "Low";
  category.value = "Work";
};
</script>

<template>
  <form class="form-card" @submit.prevent="submitTask">
    <h2>Add New Task</h2>

    <input v-model="title" type="text" placeholder="Task Title" required />

    <textarea
      v-model="description"
      placeholder="Task Description"
      rows="3"
    ></textarea>

    <select v-model="priority">
      <option value="Low">Low Priority</option>
      <option value="Medium">Medium Priority</option>
      <option value="High">High Priority</option>
    </select>

    <select v-model="category">
      <option value="Work">Work</option>
      <option value="Personal">Personal</option>
      <option value="Urgent">Urgent</option>
    </select>

    <button type="submit">Add Task</button>
  </form>
</template>

<style>
.form-card {
    margin: 0 0 15px 0;
  color: #000000;   /* Pure black */
  font-size: 20px;
  font-weight: 600;
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.form-card h2 {
  margin: 0 0 10px 0;
}

input,
textarea,
select {
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
  font-size: 14px;
}

button {
  background: #4e73df;
  color: white;
  padding: 8px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: #2e59d9;
}
</style>