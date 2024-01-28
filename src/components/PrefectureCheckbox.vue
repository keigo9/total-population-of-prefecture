<script setup lang="ts">
import { ref } from 'vue'

const checkedNames = ref<string[]>([])
</script>

<template>
  <div>
    <input type="checkbox" id="tokyo" value="Tokyo" v-model="checkedNames" />
    <label for="tokyo">Tokyo</label>
    <input type="checkbox" id="kanagawa" value="Kanagawa" v-model="checkedNames" />
    <label for="kanagawa">Kanagawa</label>
    <br />
    <span>Checked: {{ checkedNames }}</span>
  </div>
</template>

<style scoped></style>
