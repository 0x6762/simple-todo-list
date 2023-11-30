<script setup lang="ts">
import TaskList from './components/TaskList.vue'
</script>

<template>
  <h1 class="list-name">My tasks</h1>

  <TaskList></TaskList>

  <section class="wrap-add-task">
    <div class="wrap-content">
      <div class="wrap-input">
        <p class="label">New task</p>
        <input type="text" placeholder="What do you want to add?" />
      </div>
      <button class="action-btn">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
          <path d="M12 5V19" stroke="inherit" stroke-width="2" stroke-linecap="round" />
          <path d="M5 12L19 12" stroke="inherit" stroke-width="2" stroke-linecap="round" />
        </svg>
      </button>
    </div>
  </section>
</template>

<style lang="scss" scoped>
h1 {
  margin: 24px 0 24px 24px;
  font-size: 18px;
  font-weight: 600;
  color: var(--white);
}

/* ADD TASK COMPONENT */

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
</style>
