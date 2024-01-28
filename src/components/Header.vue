<script setup lang="ts">
defineProps<{
  msg: string
}>()
</script>

<template>
  <nav class="header">
    <h1>{{ msg }}</h1>
  </nav>
</template>

<style scoped>
.header {
  margin-bottom: 2.5rem;
}
.header h1 {
  font-weight: 500;
  font-size: 2.6rem;
  text-align: center;
  background-color: green;
}

@media (max-width: 1024px) {
  .header h1 {
    font-size: 1.5rem;
  }
}
</style>
