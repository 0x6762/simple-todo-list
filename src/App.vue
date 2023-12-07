<template>
  <div :class="{ 'dark-mode': isDarkMode }">
    <h1>Task List</h1>
    <form @submit.prevent="addTask" class="wrap-add-task">
      <div class="wrap-content">
        <div class="wrap-input">
          <input v-model="newTask" type="text" placeholder="Enter a new task" />
        </div>
        <button type="submit" class="action-btn">Add Task</button>
      </div>
    </form>
    <div class="wrap-list">
      <ul v-if="tasks.length > 0" class="list-data">
        <li v-for="(task, index) in tasks" :key="index" class="list-item">
          <input v-model="task.title" type="text" />
          <button @click="updateTask(task)">Update</button>
          <button @click="deleteTask(index)">Delete</button>
        </li>
      </ul>
      <div v-else class="list-empty-state">No tasks available</div>
    </div>
    <!-- <button @click="toggleDarkMode">Toggle Dark Mode</button> -->
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
h1 {
  margin: 24px 0 24px 24px;
  font-size: 18px;
  font-weight: 600;
  color: var(--white);
}

/* TASK LIST */
.wrap-list {
  display: flex;
  flex-direction: column;
  position: relative;
  padding: 8px;
  margin: 8px;
  background-color: var(--surface-color);
  border-radius: 8px;

  .list-data {
    margin-top: 8px;
  }

  .list-empty-state {
    height: 30%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--gray500);
  }
}
/* END TASK LIST */

/* ADD TASK INPUT */
.wrap-add-task {
  .wrap-content {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 8px;
  }
}
.wrap-input {
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;

  svg {
    width: 24px;
    height: 24px;
    stroke: var(--accent-color);
  }

  input {
    background-color: var(--surface-color);
    padding: 12px 16px;
    color: var(--white);
    font-size: 18px;
    font-weight: 400;
    border: none;
    border-radius: 8px;
    outline: none;
    &::placeholder {
      color: var(--gray600);
    }
    &:focus {
      &::placeholder {
        color: transparent;
      }
    }
  }
}
.action-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  background-color: var(--gray100);
  border: 1px solid var(--gray150);
  border-radius: 80px;
  cursor: pointer;

  svg {
    stroke: var(--accent-color);
  }
}
/* END ADD TASK INPUT */

.list-item {
  display: flex;
  gap: 16px;
  margin-bottom: 8px;
  background-color: var(--gray200);
  padding: 16px;
  border-radius: 6px;

  &:last-child {
    margin-bottom: 0;
  }

  input[type='checkbox'] {
    appearance: none;
    font: inherit;
    color: currentColor;
    min-width: 16px;
    width: 16px;
    height: 16px;
    border: 0.15em solid var(--gray500);
    border-radius: 0.15em;
    cursor: pointer;
    margin-top: 2px;

    display: grid;
    place-content: center;
  }

  input[type='checkbox']::before {
    content: '';
    width: 0.5em;
    height: 0.5em;
    transform: scale(0);
    transition: 100ms transform ease-in-out;
    box-shadow: inset 1em 1em var(--white);
  }

  input[type='checkbox']:checked::before {
    transform: scale(1);
  }

  .content {
    flex: 1;
    font-size: 16px;
  }

  svg {
    stroke: var(--gray600);
    width: 16px;
    height: 16px;
    margin-top: 2px;
    cursor: pointer;

    &:hover {
      stroke: var(--white);
    }
  }
}
</style>
