
<script setup>
import { ref, onMounted, watch } from "vue";

const newTask = ref("");
const tasks = ref([]);
const showAddNotification = ref(false);
const addNotificationMessage = ref("");
const showRemoveNotification = ref(false);
const removeNotificationMessage = ref("");

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
    saveTasks();
    showAddNotification.value = true;
    addNotificationMessage.value = "Task added successfully.";
    setTimeout(() => {
      showAddNotification.value = false;
    }, 1000); 

  } else {
    showAddNotification.value = true;
    addNotificationMessage.value = "Please enter a task.";
    setTimeout(() => {
      showAddNotification.value = false;
    }, 1000);  
  }
};

const removeTask = (index) => {
  const removedTask = tasks.value[index];
  tasks.value.splice(index, 1);
  saveTasks();
  showRemoveNotification.value = true;
  removeNotificationMessage.value = `"${removedTask}" removed successfully.`;
  setTimeout(() => {
    showRemoveNotification.value = false;
  }, 1000); 
};

const saveTasks = () => {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

const loadTasks = () => {
  const savedTasks = localStorage.getItem("tasks");
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
};

onMounted(() => {
  loadTasks();
});

watch(tasks, () => {
  saveTasks();
});
</script>

<template>
  <div class="todo-app">
    <h1 class="app-title">Vue To-Do List</h1>
    <div class="task-input">
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />
      <button @click="addTask">Add</button>
    </div>

    <transition name="fade">
      <div v-if="showAddNotification" class="notification success">
        {{ addNotificationMessage }}
      </div>
    </transition>

    <ul class="task-list">
      <transition-group name="fade" tag="div">
        <li v-for="(task, index) in tasks" :key="index" class="task-item">
          <span>{{ task }}</span>
          <button @click="removeTask(index)" class="remove-button">Remove</button>
        </li>
      </transition-group>
    </ul>

    <transition name="fade">
      <div v-if="showRemoveNotification" class="notification danger">
        {{ removeNotificationMessage }}
      </div>
    </transition>
  </div>
</template>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 80px auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.app-title {
  font-size: 24px;
  margin-bottom: 20px;
  text-align: center;
  color: #333;
}

.task-input {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 8px 12px;
  font-size: 14px;
  background-color: #4caf80;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #45a168;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin: 8px 0;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 4px;
  transition: background-color 0.3s ease;
}

.task-item:hover {
  background-color: #f0f0f0;
}

.remove-button {
  padding: 6px 10px;
  font-size: 12px;
  background-color: #e74c3c;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.remove-button:hover {
  background-color: #c0392b;
}

.notification {
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 20px;
}

.success {
  background-color: #4caf80;
  color: #fff;
}
.Info {
  background-color: #f89807;
  color: #fff;
}
.danger {
  background-color: #e74c3c;
  color: #fff;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
</style>
