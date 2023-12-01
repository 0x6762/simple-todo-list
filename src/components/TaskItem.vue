<script setup lang="ts">
defineProps(['title'])
defineEmits(['removeTask'])
</script>

<template>
  <li class="list-item">
    <input type="checkbox" />
    <p class="content">{{ title }}</p>
    <button @click="$emit('removeTask')" class="remove-item">X</button>
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

  button {
    cursor: pointer;
  }
}
</style>
