<template>
  <div :class="{ 'dark-mode': isDarkMode }">
    <h1>Task List</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTask" type="text" placeholder="Enter a new task" />
      <button type="submit">Add Task</button>
    </form>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <input v-model="task.title" type="text" />
        <button @click="updateTask(task)">Update</button>
        <button @click="deleteTask(index)">Delete</button>
      </li>
    </ul>
    <button @click="toggleDarkMode">Toggle Dark Mode</button>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Task {
  title: string
}

const tasks = ref<Task[]>([])
const newTask = ref('')
const isDarkMode = ref(false)

function addTask() {
  if (newTask.value.trim() !== '') {
    tasks.value.push({ title: newTask.value })
    newTask.value = ''
    saveTasksToLocalStorage()
  }
}

function updateTask(task: Task) {
  task.title = task.title.trim()
  saveTasksToLocalStorage()
}

function deleteTask(index: number) {
  tasks.value.splice(index, 1)
  saveTasksToLocalStorage()
}

function saveTasksToLocalStorage() {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

function loadTasksFromLocalStorage() {
  const savedTasks = localStorage.getItem('tasks')
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks)
  }
}

function toggleDarkMode() {
  isDarkMode.value = !isDarkMode.value
}

onMounted(() => {
  loadTasksFromLocalStorage()
})
</script>

<style lang="scss" scoped>
// BEGIN: be15d9bcejpp
/* Material Design Colors */
$primary-color: #2196f3;
$secondary-color: #f50057;
$background-color: #f5f5f5;
$dark-background-color: #333333;

/* Material Design Typography */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500&display=swap');
$font-family: 'Inter', sans-serif;

$font-size: 16px;
$font-weight-light: 300;
$font-weight-regular: 400;
$font-weight-medium: 500;

/* Material Design Button */
$button-border-radius: 4px;
$button-padding: 8px 16px;
$button-primary-background-color: $primary-color;
$button-primary-color: #ffffff;
$button-secondary-background-color: $secondary-color;
$button-secondary-color: #ffffff;

/* Material Design Input */
$input-border-radius: 4px;
$input-border-color: #bdbdbd;
$input-focus-border-color: $primary-color;
$input-padding: 8px;
$input-font-size: $font-size;

/* Material Design List */
$list-item-padding: 8px;
$list-item-border-radius: 4px;
$list-item-background-color: #ffffff;
$list-item-dark-background-color: #222222;

/* Material Design Card */
$card-border-radius: 4px;
$card-background-color: #ffffff;
$card-dark-background-color: #222222;
$card-box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

/* Apply Material Design styles */
body {
  background-color: $background-color;
  font-family: $font-family;
  font-size: $font-size;
  font-weight: $font-weight-regular;
}

.dark-mode {
  background-color: $dark-background-color;
}

button {
  border-radius: $button-border-radius;
  padding: $button-padding;
  font-family: $font-family;
  font-size: $font-size;
  font-weight: $font-weight-medium;
}

input {
  border-radius: $input-border-radius;
  border: 1px solid $input-border-color;
  padding: $input-padding;
  font-size: $input-font-size;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  padding: $list-item-padding;
  border-radius: $list-item-border-radius;
  background-color: $list-item-background-color;
  box-shadow: $card-box-shadow;
}

.card {
  border-radius: $card-border-radius;
  background-color: $card-background-color;
  box-shadow: $card-box-shadow;
}

.dark-mode button,
.dark-mode input,
.dark-mode li {
  background-color: $list-item-dark-background-color;
  color: #ffffff;
}

.dark-mode .card {
  background-color: $card-dark-background-color;
}
</style>
