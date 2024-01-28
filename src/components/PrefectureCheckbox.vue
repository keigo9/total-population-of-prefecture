<script setup lang="ts">
import { ref } from 'vue'

const apiKey = process.env.VITE_API_KEY || ""
const endpointRoot = process.env.VITE_API_URL
// console.log(apiKey)
// console.log(endpointRoot)

const prefectures = ref<string[]>([])
const checkedPrefectures = ref<string[]>([])

async function fetchData() {
  try {
    const res = await fetch(
      `${endpointRoot}/api/v1/prefectures`,
      {headers: {"x-api-key": apiKey}}
    )
    const prefecturesData = await res.json()
    prefectures.value = prefecturesData.result.map((val => ({ ...val, isChecked: false })))
    console.log(prefectures.value)
  } catch (error) {
    console.error(error);
  }
}

fetchData()
</script>

<template>
  <h2>都道府県</h2>
  <div class="checkbox-container">
    <div class="checkbox-wrapper" v-for="prefecture in prefectures" :key="prefecture.id">
      <input type="checkbox" :id="prefecture.prefCode" :value="prefecture" :checked="prefecture.isChecked" v-model="checkedPrefectures" />
      <label :for="prefecture.prefCode">{{ prefecture.prefName }}</label>
    </div>
  </div>
  <br>
  <span>Checked: {{ checkedPrefectures }}</span>
</template>

<style scoped>
h2 {
  margin-bottom: 1rem;
}
.checkbox-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px 15px;;
}
.checkbox-wrapper {
  text-wrap: nowrap;
  display: flex;
  gap: 2px;
  justify-content: center;
  align-items: center;
}
@media (max-width: 1024px) {
  h2 {
    font-size: 1.3rem;
  }
}
</style>
