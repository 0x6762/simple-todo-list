<script setup lang="ts">
import CloseIcon from './icons/ic_close.svg'
defineProps(['title'])
defineEmits(['removeTask'])
</script>

<template>
  <li class="list-item">
    <input type="checkbox" />
    <p class="content">{{ title }}</p>
    <CloseIcon @click="$emit('removeTask')" class="remove-item" />
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
