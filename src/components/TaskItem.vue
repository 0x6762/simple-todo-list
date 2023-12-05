<script setup lang="ts">
defineProps(['title'])
defineEmits(['removeTask'])
</script>

<template>
  <li class="list-item">
    <input type="checkbox" />
    <p class="content">{{ title }}</p>
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
  align-items: baseline;
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

  button {
    background-color: transparent;
    cursor: pointer;

    svg {
      stroke: var(--gray500);
    }

    &:hover {
      svg {
        stroke: var(--white);
      }
    }
  }
}
</style>
