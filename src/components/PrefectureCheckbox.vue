<script setup lang="ts">
import { ref, defineEmits } from 'vue'

const apiKey = process.env.VITE_API_KEY || ""
const endpointRoot = process.env.VITE_API_URL
// console.log(apiKey)
// console.log(endpointRoot)

interface Prefecture {
  prefCode: number;
  prefName: string;
  isChecked: boolean;
}

const prefectures = ref<Prefecture[]>([])
const checkedPrefectures = ref<Prefecture[]>([])

interface PrefectureResultItem {
  prefCode: number;
  prefName: string;
}
interface populationResultItem {
  year: number;
  value: number;
}

async function fetchPrefectureData() {
  try {
    const res = await fetch(
      `${endpointRoot}/api/v1/prefectures`,
      {headers: {"x-api-key": apiKey}}
    )
    const prefecturesData = await res.json()
    prefectures.value = prefecturesData.result.map(((val: PrefectureResultItem) => ({ ...val, isChecked: false })))
    // console.log(prefectures.value)
  } catch (error) {
    console.error(error)
  }
}

const emit = defineEmits(['onAddSeries', 'onRemoveSeries'])

async function drawChart(id: number, name: string) {
  // prefCodeを元に、県の人口推移を取得
  try {
     const res = await fetch(
      `${endpointRoot}/api/v1/population/composition/perYear?cityCode=-&prefCode=${id}`,
      {headers: {"x-api-key": apiKey}}
    )
    const data = await res.json()
    // console.log(data)
    const populationData = data.result.data[0].data.map((item: populationResultItem) => item.value)
    // console.log(data)
    // console.log(populationData)
    // chartにデータを追加、親の関数を発火
    emit("onAddSeries", id, name, populationData)
    prefectures.value[id - 1].isChecked = true
  } catch (error) {
    console.error(error)
  }
}

function deleteChart(id: number) {
  emit("onRemoveSeries", id)
  prefectures.value[id - 1].isChecked = false
}

function toggleShowChart(id: number, name: string, isChecked: boolean) {
  if (isChecked) {
    deleteChart(id)
  } else {
   drawChart(id, name)
  }
}

fetchPrefectureData()
</script>

<template>
  <h2>都道府県</h2>
  <div class="checkbox-container">
    <div class="checkbox-wrapper" v-for="prefecture in prefectures" :key="prefecture.prefCode">
      <input type="checkbox" :id="String(prefecture.prefCode)" :value="prefecture" :checked="prefecture.isChecked" @click="toggleShowChart(prefecture.prefCode, prefecture.prefName, prefecture.isChecked)" />
      <label :for="String(prefecture.prefCode)">{{ prefecture.prefName }}</label>
    </div>
  </div>
  <br>
  <!-- <span>Checked: {{ checkedPrefectures }}</span> -->
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
