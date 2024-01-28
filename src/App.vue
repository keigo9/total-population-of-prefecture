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
