<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <!-- LIST COMPONENT -->
  <section class="wrap-list">
    <ul class="list-data">
      <li class="list-item">
        <input type="checkbox" />
        <p class="content">This is an item</p>
      </li>
    </ul>
  </section>
  <section class="wrap-add-task">
    <div class="wrap-input">
      <p class="label">New task</p>
      <input type="text" placeholder="What do you want to add?" />
    </div>
    <div class="action-btn">+</div>
  </section>
  <!-- LIST COMPONENT -->
</template>

<style lang="scss">
/* LIST COMPONENT */
.wrap-list {
  height: 80vh;
  padding: 24px;
  margin: 8px;
  background-color: var(--surface-color);
  border-radius: 8px;

  .list-item {
    display: flex;
  }
}

/* ADD TASK COMPONENT */
.wrap-input {
  position: sticky;
  bottom: 8px;
  padding: 8px;

  .action-btn {
    background-color: --surface-color;
  }
}
</style>
