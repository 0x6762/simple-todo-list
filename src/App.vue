<script setup lang="ts">
import { ref } from 'vue'
import TaskItem from './components/TaskItem.vue'

defineProps(['title'])

const tasks = ref([
  // { id: 1, title: 'Learn to write in local storage' },
  // { id: 2, title: 'Check pinia' },
  // { id: 3, title: 'Fix localhost for external access' }
])

const addTask = () => {
  tasks.value.push({ id: tasks.value.length + 1, title: newItem.value })
  newItem.value = ''
}

const newItem = ref('')
</script>

<template>
  <h1 class="list-name">My tasks</h1>

  <!-- TASK LIST -->
  <section class="wrap-list">
    <ul class="list-data">
      <TaskItem v-for="item in tasks" :key="item.id" :title="item.title"> </TaskItem>
    </ul>
    <div class="list-empty-state" v-if="!tasks.length">
      <p>Nothing here yet</p>
    </div>
  </section>
  <!-- END TASK LIST -->

  <!-- ADD TASK INPUT -->
  <section class="wrap-add-task">
    <form class="wrap-content" @submit.prevent="addTask">
      <div class="wrap-input">
        <p class="label">New task</p>
        <input v-model.trim="newItem" type="text" placeholder="What do you want to add?" />
      </div>
      <button class="action-btn">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
          <path d="M12 5V19" stroke="inherit" stroke-width="2" stroke-linecap="round" />
          <path d="M5 12L19 12" stroke="inherit" stroke-width="2" stroke-linecap="round" />
        </svg>
      </button>
    </form>
  </section>
  <!-- END ADD TASK INPUT -->
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
  height: 70vh;
  padding: 8px;
  margin: 8px;
  background-color: var(--surface-color);
  border-radius: 8px;

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
  position: fixed;
  bottom: 8px;
  left: 0;
  right: 0;

  .wrap-content {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 24px;
  }
}
.wrap-input {
  .label {
    margin-bottom: 8px;
    font-size: 14px;
  }
  input {
    width: 100%;
    background-color: var(--bg-color);
    color: var(--accent-color);
    font-size: 18px;
    font-weight: 400;
    border: none;
    outline: none;
    &::placeholder {
      color: var(--gray500);
    }
    &:focus {
      &::placeholder {
        color: var(--bg-color);
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
