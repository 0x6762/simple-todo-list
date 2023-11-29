<script setup lang="ts">
import { ref } from 'vue'
import TaskItem from './TaskItem.vue'
defineProps(['title'])

const posts = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
])
</script>

<template>
  <!-- LIST COMPONENT -->
  <section class="wrap-list">
    <ul class="list-data">
      <TaskItem v-for="post in posts" :key="post.id" :title="post.title"></TaskItem>
    </ul>
  </section>
</template>

<style lang="scss">
/* LIST COMPONENT */
.wrap-list {
  height: 70vh;
  padding: 24px;
  margin: 8px;
  background-color: var(--surface-color);
  border-radius: 8px;

  .list-item {
    display: flex;
    gap: 16px;

    p {
      font-size: 16px;
    }
  }
}
</style>
