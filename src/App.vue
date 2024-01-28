<script setup lang="ts">
import Header from './components/Header.vue'
import PrefectureCheckbox from './components/PrefectureCheckbox.vue'
</script>

<template>
  <header>
    <Header msg="都道府県別の総人口推移" />
  </header>

  <main>
    <PrefectureCheckbox />
  </main>
</template>

<style scoped>
main {
  margin: 0 auto;
  max-width: 1000px;
  padding: 0 20px;
}
</style>