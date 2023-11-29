<script setup lang="ts">
defineProps(['title'])
</script>

<template>
  <li class="list-item">
    <input type="checkbox" />
    <p class="content">{{ title }}</p>
  </li>
</template>

<style lang="scss"></style>
