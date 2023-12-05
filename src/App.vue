<script setup lang="ts">
import { ref, onMounted } from 'vue'
import TaskItem from './components/TaskItem.vue'

import ArrowIcon from './components/icons/ic_arrow.svg'

defineProps(['title'])

const STORAGE_KEY = 'todo-app-storage'
onMounted(() => {
  tasks.value = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
})

// Sets type to item
type task = {
  id: number
  title: string
}

// Sets an empty array to receive task list
const tasks = ref<task[]>([])
// Sets an empty string to receive input value
const taskText = ref('')

// Add task to list
const addTask = () => {
  tasks.value.push({ id: tasks.value.length + 1, title: taskText.value })
  taskText.value = ''
  // Add task to local storage
  localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks.value))
}
// Remove task from list
const removeTask = (task) => {
  tasks.value.splice(tasks.value.indexOf(task), 1)
  localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks.value))
}
</script>

<template>
  <div class="main">
    <header>
      <h1 class="list-name">My tasks</h1>
    </header>

    <!-- TASK LIST -->
    <section class="wrap-list">
      <ul class="list-data" v-if="tasks.length != 0">
        <TaskItem
          v-for="task in tasks"
          :key="task.id"
          :title="task.title"
          @remove-task="removeTask(task)"
        >
        </TaskItem>
      </ul>
      <!-- <div class="list-empty-state" v-if="!tasks.length">
        <p>Nothing here yet</p>
      </div> -->

      <!-- ADD TASK INPUT -->
      <section class="wrap-add-task">
        <form class="wrap-content" @submit.prevent="addTask">
          <div class="wrap-input">
            <ArrowIcon />
            <input v-model.trim="taskText" type="text" placeholder="Add a new task" />
          </div>
        </form>
      </section>
      <!-- END ADD TASK INPUT -->
    </section>
    <!-- END TASK LIST -->
  </div>
</template>

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
  flex-direction: column-reverse;
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
</style>
