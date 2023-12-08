<template>
  <li class="list-item">
    <input type="checkbox" />
    <template v-if="!isEditing.value">
      <p class="content">{{ task.title }}</p>
      <CloseIcon @click="$emit('removeTask')" class="remove-item" />
      <EditIcon @click="startEditingTask" class="edit-item" />
    </template>
    <template v-else>
      <input
        v-model="editedTitle"
        class="edit-input"
        @keyup.enter="updateTask"
        @blur="cancelEditingTask"
      />
      <CloseIcon @click="cancelEditingTask" class="cancel-item" />
    </template>
  </li>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue' // Import reactive here

import CloseIcon from './icons/ic_close.svg'
import EditIcon from './icons/ic_arrow.svg'

const props = defineProps({
  task: Object,
  isTaskEditing: Boolean
})

const emit = defineEmits(['removeTask', 'startEditingTask', 'updateTask', 'cancelEditingTask'])

const { editedTitle } = reactive({
  editedTitle: props.task.title
})

const isEditing = ref(false) // Separate ref for isEditing state

const startEditingTask = () => {
  emit('startEditingTask', props.task)
  isEditing.value = true
}

const cancelEditingTask = () => {
  editedTitle.value = props.task.title
  emit('cancelEditingTask')
  isEditing.value = false
}

const updateTask = () => {
  emit('updateTask', { ...props.task, title: editedTitle.value })
  isEditing.value = false
}
</script>

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
