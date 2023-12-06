<template>
  <div class="main">
    <header>
      <h1 class="list-name">My tasks</h1>
    </header>

    <section class="wrap-list">
      <ul class="list-data" v-if="tasks.length != 0">
        <li class="list-item" v-for="task in tasks" :key="task.id">
          <input type="checkbox" />
          <template v-if="!task.isEditing">
            <p class="content">{{ task.title }}</p>
            <CloseIcon @click="removeTask(task)" class="remove-item" />
            <EditIcon @click="startEditingTask(task)" class="edit-item" />
          </template>
          <template v-else>
            <input
              v-model="task.title"
              class="edit-input"
              @keyup.enter="updateTask({ ...task, title: $event.target.value })"
              @blur="cancelEditingTask(task)"
            />
            <CloseIcon @click="cancelEditingTask(task)" class="cancel-item" />
          </template>
        </li>
      </ul>
    </section>

    <section class="wrap-add-task">
      <form class="wrap-content" @submit.prevent="addTask">
        <div class="wrap-input">
          <ArrowIcon />
          <input v-model.trim="taskText" type="text" placeholder="Add a new task" />
        </div>
      </form>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

import CloseIcon from './components/icons/ic_close.svg'
import EditIcon from './components/icons/ic_arrow.svg'

const STORAGE_KEY = 'todo-app-storage'

onMounted(() => {
  tasks.value = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
})

// Define interface for task
type task = {
  id: number
  title: string
  isEditing: boolean
}

// Define reactive state variables
const tasks = ref<task[]>([])
const taskText = ref('')

// Add task to list
const addTask = () => {
  tasks.value.push({
    id: tasks.value.length + 1,
    title: taskText.value,
    isEditing: false
  })
  taskText.value = ''
  localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks.value))
}

// Remove task from list
const removeTask = (task: task) => {
  tasks.value = tasks.value.filter((item) => item !== task)
  localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks.value))
}

// Start editing a task
const startEditingTask = (task: task) => {
  task.isEditing = true
}

// Cancel editing a task
const cancelEditingTask = () => {
  tasks.value.forEach((task) => {
    task.isEditing = false
  })
}

// Update a task
const updateTask = (task: task) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks.value))
}
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
