<script setup lang="ts">
defineProps(['title'])
defineEmits(['removeTask'])
</script>

<template>
  <li class="list-item">
    <div class="wrap-content">
      <input type="checkbox" />
      <p class="content">{{ title }}</p>
    </div>
    <button @click="$emit('removeTask')" class="remove-item">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
        <path
          d="M18 6L6 18"
          stroke="inherit"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M6 6L18 18"
          stroke="inherit"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
    </button>
  </li>
</template>

<style lang="scss">
.list-item {
  display: flex;
  gap: 16px;
  justify-content: space-between;
  align-items: baseline;
  margin-bottom: 8px;
  background-color: var(--gray200);
  padding: 16px;
  border-radius: 6px;

  .wrap-content {
    display: flex;
    align-items: baseline;
    gap: 16px;

    p {
      font-size: 16px;
    }
  }

  button {
    background-color: transparent;
    cursor: pointer;

    svg {
      stroke: var(--gray500);
    }
  }
}
</style>
