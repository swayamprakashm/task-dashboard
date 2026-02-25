<script setup>
import { ref, onMounted, watch, computed } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";

const tasks = ref([]);
const selectedCategory = ref("All");

onMounted(() => {
  const savedTasks = localStorage.getItem("tasks");
  if (savedTasks) tasks.value = JSON.parse(savedTasks);
});

watch(
  tasks,
  (newTasks) => {
    localStorage.setItem("tasks", JSON.stringify(newTasks));
  },
  { deep: true }
);

const filteredTasks = computed(() => {
  if (selectedCategory.value === "All") return tasks.value;
  return tasks.value.filter(
    (task) => task.category === selectedCategory.value
  );
});

const addTask = (task) => tasks.value.push(task);

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

const updateTask = (updatedTask) => {
  const index = tasks.value.findIndex(t => t.id === updatedTask.id);
  if (index !== -1) tasks.value[index] = updatedTask;
};
</script>

<template>
  <div class="dashboard">
    <header class="header">
      📋 Task Management Dashboard
    </header>

    <div class="wrapper">
      <div class="content">
        <div class="left-panel">
          <TaskForm @add-task="addTask" />
        </div>

        <div class="right-panel">
          <div class="filter-box">
            <label>Filter:</label>
            <select v-model="selectedCategory">
              <option value="All">All</option>
              <option value="Work">Work</option>
              <option value="Personal">Personal</option>
              <option value="Urgent">Urgent</option>
            </select>
          </div>

          <TaskList
            :tasks="filteredTasks"
            @delete-task="deleteTask"
            @update-task="updateTask"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Remove default Vite spacing */
html, body {
  margin: 0;
  padding: 0;
}

/* Full page background */
body {
  font-family: "Segoe UI", sans-serif;
  background: linear-gradient(135deg, #eef2f7, #dbe4f3);
}

/* Main container */
.dashboard {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Full width header */
.header {
  width: 100%;
  background: linear-gradient(135deg, #4e73df, #224abe);
  color: white;
  padding: 25px 0;
  text-align: center;
  font-size: 26px;
  font-weight: 600;
}

/* Centering wrapper */
.wrapper {
  flex: 1;
  display: flex;
  justify-content: center;   /* 🔥 THIS centers horizontally */
  align-items: flex-start;
  padding: 40px 20px;
}

/* Main content box */
.content {
  width: 100%;
  max-width: 1100px;   /* controls total width */
  display: flex;
  gap: 30px;
}

/* Panels */
.left-panel {
  flex: 1;
}

.right-panel {
  flex: 1.5;
}

.filter-box {
  margin-bottom: 20px;
}

select {
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
  margin-left: 10px;
}

/* Mobile */
@media (max-width: 900px) {
  .content {
    flex-direction: column;
  }
}
</style>