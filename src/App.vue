<template>
  <!-- <div :class="{ 'dark-mode': isDarkMode }"> -->
  <div>
    <h1>Task List</h1>
    <div class="wrap-list">
      <form @submit.prevent="addTask" class="wrap-add-task">
        <div class="wrap-content">
          <div class="wrap-input">
            <ArrowIcon />
            <input v-model="newTask" type="text" placeholder="Enter a new task" />
          </div>
        </div>
      </form>
      <ul v-if="activeTasks.length > 0 || completedTasks.length > 0" class="list-data">
        <div class="list-effect">
          <transition-group name="list">
            <li v-for="(task, index) in activeTasks" :key="task.id" class="list-item">
              <input
                type="checkbox"
                class="checkbox"
                :checked="task.completed"
                @change="completeTask(task)"
              />
              <input
                :class="{ content: true, completed: task.completed }"
                v-model="task.title"
                type="text"
                @keyup.enter="updateTask(task), $event.target.blur()"
                @blur="updateTask(task)"
              />
              <CloseIcon @click="deleteTask(index)" />
            </li>
          </transition-group>
        </div>
        <!-- Completed tasks -->
        <p class="completed-task-title" v-if="completedTasks.length > 0">Completed tasks</p>
        <li v-for="(task, index) in completedTasks" :key="task.id" class="list-item">
          <input
            type="checkbox"
            class="checkbox"
            :checked="task.completed"
            @change="completeTask(task)"
          />
          <p :class="{ content: true, completed: task.completed }">{{ task.title }}</p>
        </li>
        <!-- End Completed tasks -->
      </ul>
      <div v-else class="list-empty-state">No tasks available</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import ArrowIcon from './components/icons/ic_arrow.svg'
import CloseIcon from './components/icons/ic_close.svg'

interface Task {
  title: string
  completed: boolean
}

const activeTasks = ref<Task[]>([])
const completedTasks = ref<Task[]>([])
const newTask = ref('')

// const isDarkMode = ref(false)

function addTask() {
  const task = {
    id: Date.now(), // Use the current timestamp as a unique id
    title: newTask.value,
    completed: false
  }
  activeTasks.value.push(task)
  newTask.value = ''
  orderTasksById()
  saveTasksToLocalStorage()
}

function updateTask(task: Task) {
  task.title = task.title.trim()
  saveTasksToLocalStorage()
}

function deleteTask(index: number) {
  activeTasks.value.splice(index, 1)
  saveTasksToLocalStorage()
}

function completeTask(task: Task) {
  task.completed = !task.completed
  if (task.completed) {
    const index = activeTasks.value.indexOf(task)
    activeTasks.value.splice(index, 1)
    completedTasks.value.push(task)
  } else {
    const index = completedTasks.value.indexOf(task)
    completedTasks.value.splice(index, 1)
    activeTasks.value.unshift(task)
    orderTasksById()
  }
  saveTasksToLocalStorage()
}

function orderTasksById() {
  activeTasks.value.sort((a, b) => a.id - b.id)
}

function saveTasksToLocalStorage() {
  localStorage.setItem('activeTasks', JSON.stringify(activeTasks.value))
  localStorage.setItem('completedTasks', JSON.stringify(completedTasks.value))
}

function loadTasksFromLocalStorage() {
  const savedActiveTasks = localStorage.getItem('activeTasks')
  const savedCompletedTasks = localStorage.getItem('completedTasks')
  if (savedActiveTasks) {
    activeTasks.value = JSON.parse(savedActiveTasks)
  }
  if (savedCompletedTasks) {
    completedTasks.value = JSON.parse(savedCompletedTasks)
  }
}

// function toggleDarkMode() {
//   isDarkMode.value = !isDarkMode.value
// }

onMounted(() => {
  loadTasksFromLocalStorage()
})
</script>

<style lang="scss" scoped>
h1 {
  margin: 24px 0 24px 24px;
}

.wrap-list {
  display: flex;
  flex-direction: column;
  position: relative;
  padding: 8px;
  margin: 8px;
  background-color: var(--surface-color);
  border-radius: 8px;
}

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

.completed-task-title {
  font-size: 14px;
  margin-top: 24px;
  margin-bottom: 16px;
  color: var(--gray900);
}

.list-effect {
  .list-enter-active,
  .list-leave-active {
    transition: all 0.3s ease;
  }
  .list-enter-from,
  .list-leave-to {
    opacity: 0;
    transform: translateY(10px);
  }
}

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

  .checkbox {
    margin-right: 8px;
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
    background-color: transparent;
    color: var(--white);
    border: none;
    outline: none;
  }

  .completed {
    text-decoration: line-through;
    color: var(--gray500);
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
