<script setup lang="ts">
import { ref } from 'vue'
import TaskItem from './TaskItem.vue'
defineProps(['title'])
defineEmits(['addTask'])

const tasks = ref([
  { id: 1, title: 'Learn to write in local storage' },
  { id: 2, title: 'Check pinia' },
  { id: 3, title: 'Fix localhost for external access' }
])

const addTask = () => {
  tasks.value.push({ id: 4, title: 'New task' })
}
</script>

<template>
  <section class="wrap-list">
    <ul class="list-data">
      <TaskItem v-for="item in tasks" :key="item.id" :title="item.title"> </TaskItem>
    </ul>
    <button @click="addTask">Add</button>
  </section>
</template>

<style lang="scss">
.wrap-list {
  height: 70vh;
  padding: 8px;
  margin: 8px;
  background-color: var(--surface-color);
  border-radius: 8px;
}
</style>
