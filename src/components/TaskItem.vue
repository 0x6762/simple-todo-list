<script setup lang="ts">
defineProps(['title'])
</script>

<template>
  <li class="list-item">
    <input type="checkbox" />
    <p class="content">{{ title }}</p>
  </li>
</template>

<style lang="scss">
.list-item {
  display: flex;
  gap: 16px;
  margin-bottom: 8px;
  background-color: var(--gray200);
  padding: 16px;
  border-radius: 6px;

  p {
    font-size: 16px;
  }
}
</style>
