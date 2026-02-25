<script setup>
import { ref } from "vue";
import EditModal from "./EditModal.vue";

const props = defineProps(["task"]);
const emit = defineEmits(["delete-task", "update-task"]);

const showModal = ref(false);

const toggleStatus = () => {
  const updatedTask = { ...props.task };
  updatedTask.status =
    updatedTask.status === "Pending" ? "Completed" : "Pending";
  emit("update-task", updatedTask);
};

const deleteTask = () => {
  if (confirm("Are you sure you want to delete this task?")) {
    emit("delete-task", props.task.id);
  }
};

const openEdit = () => {
  showModal.value = true;
};
</script>

<template>
  <div :class="['task-card', task.status === 'Completed' ? 'completed' : '']">
    <h3>{{ task.title }}</h3>
    <p>{{ task.description }}</p>

    <div class="task-info">
      <span><strong>Priority:</strong> {{ task.priority }}</span>
      <span><strong>Category:</strong> {{ task.category }}</span>
      <span><strong>Status:</strong> {{ task.status }}</span>
    </div>

    <div class="btn-group">
      <button class="btn toggle" @click="toggleStatus">Toggle</button>
      <button class="btn edit" @click="openEdit">Edit</button>
      <button class="btn delete" @click="deleteTask">Delete</button>
    </div>

    <EditModal
      v-if="showModal"
      :task="task"
      @close="showModal = false"
      @save="emit('update-task', $event)"
    />
  </div>
</template>

<style>
.task-card {
margin: 0 0 15px 0;
  color: #000000;   /* Pure black */
  font-size: 20px;
  font-weight: 600;
  background: white;
  padding: 16px;
  margin-bottom: 16px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
  transition: 0.3s ease;
}

.task-card:hover {
  transform: translateY(-4px);
}

.completed {
  background: #e6f4ea;
  text-decoration: line-through;
}

.task-info {
  margin: 10px 0;
  display: flex;
  flex-direction: column;
  gap: 4px;
  font-size: 14px;
  color: #555;
}

.btn-group {
  margin-top: 10px;
}

.btn {
  margin-right: 8px;
  padding: 6px 14px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: white;
  font-size: 13px;
  transition: 0.3s;
}

.toggle {
  background: #17a673;
}

.toggle:hover {
  background: #13855c;
}

.edit {
  background: #f6c23e;
}

.edit:hover {
  background: #dda20a;
}

.delete {
  background: #e74a3b;
}

.delete:hover {
  background: #c0392b;
}
</style>