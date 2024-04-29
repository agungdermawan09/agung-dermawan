<template>
  <div class="container">
    <div class="todo-app">
      <h1>My Daily Habits</h1>
      <div class="row">
        <input type="text" v-model="newTask" placeholder="Add a Habit">
        <button @click="addTask">+</button>
      </div>

      <div class="filters">
        <button @click="toggleFilter" :class="{ active: hideCompleted }">Hide Completed</button>
      </div>

      <ul class="todo-list">
        <li v-for="(task, index) in filteredTodos" :key="task.id" :class="{ checked: task.checked }" @click="toggleTask(task)">
          <span>{{ task.text }}</span>
          <span class="delete-button" @click.stop="removeTask(task)">❌</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? tasks.value.filter((task) => !task.checked)
    : tasks.value;
});

function addTask() {
  if (newTask.value.trim() === '') {
    alert("Please fill in the task!");
  } else {
    tasks.value.unshift({ id: Date.now(), text: newTask.value, checked: false });
    newTask.value = '';
    saveData();
  }
}

function toggleTask(task) {
  task.checked = !task.checked; 
  saveData();
}

function removeTask(task) {
  const index = tasks.value.findIndex((t) => t.id === task.id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
    saveData();
  }
}

function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

function loadData() {
  const savedTasks = localStorage.getItem("tasks");
  tasks.value = savedTasks ? JSON.parse(savedTasks) : [];
}
loadData();

function toggleFilter() {
  hideCompleted.value = !hideCompleted.value;
}
</script>

<style scoped>
.container {
  background: linear-gradient(45deg, #FF6B6B, #FFA69E);
  width: 100%;
  min-height: 100vh;
  padding: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.todo-app {
  width: 100%;
  max-width: 700px;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.todo-app h1 {
  text-align: center;
  color: #333;
  font-size: 32px;
  margin-bottom: 20px;
}

.row {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

input {
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
  padding: 10px;
  font-size: 18px;
  color: #333;
  border-bottom: 2px solid #FF6B6B;
}

button {
  background: #FF6B6B;
  color: #FFF;
  border: none;
  outline: none;
  padding: 10px 20px;
  margin-left: 10px;
  font-size: 20px;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s, transform 0.2s;
}

button:hover {
  background: #FF8E8E;
  transform: scale(1.05);
}

.filters {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.filters button {
  background: transparent;
  border: none;
  outline: none;
  padding: 10px 20px;
  margin: 0 5px;
  font-size: 16px;
  color: #FF6B6B;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s;
}

.filters button.active {
  background: #FF6B6B;
  color: #FFF;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-list li {
  font-size: 20px;
  padding: 15px;
  border-bottom: 1px solid #eee;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  transition: background 0.3s;
}

.todo-list li:hover {
  background: #FFA69E;
}

.todo-list li.checked {
  text-decoration: line-through;
  color: #888;
}

.delete-button {
  font-size: 24px;
  color: #FF6B6B;
  cursor: pointer;
}
</style>
