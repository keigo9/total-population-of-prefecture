<script setup lang="ts">
import Header from './components/Header.vue'
import PrefectureCheckbox from './components/PrefectureCheckbox.vue'
// import PopulationChart from './components/PopulationChart.vue'
import { Chart } from 'highcharts-vue';
import { ref, computed } from 'vue';

interface SeriesDataItem {
  id: number;
  name: string;
  data: number[];
}
const seriesData = ref<SeriesDataItem[]>([]);
const categories = ref([ 
  1960, 1965, 1970, 1975, 1980, 1985, 1990, 1995, 2000, 2005, 2010, 
  2015, 2020, 2025, 2030, 2035, 2040, 2045,
]);

const chartOptions = computed(() => ({
  chart: {
    type: 'line',
  },
  title: {
    style: {
      display: "none",
    },
    text: 'Number of project stars',
  },
  legend: {
    align: "right",
    verticalAlign: "top",
    layout: "vertical",
  },
  xAxis: {
    categories: categories.value,
    title: {
      text: '年度',
    },
  },
  yAxis: {
    title: {
      text: '人口数',
    },
  },
  series: seriesData.value,
  responsive: {
  rules: [{
    condition: {
      maxWidth: 500
    },
    chartOptions: {
      legend: {
        // enabled: false
        align: "center",
        verticalAlign: "top",
        alignColumns: false,
        layout: "horizontal",
      }
    }
  }]
}
}));

const addSeries = (id: number, name: string, population: number[]) => {
  // console.log(id, name, population)
  // console.log("addSerires on fire")
  seriesData.value = [...seriesData.value, {
    id,
    name,
    data: population,
  }];
};

const removeSeries = (id: number) => {
  // console.log("removeSerires on fire")
  seriesData.value = seriesData.value.filter(val => val.id !== id);
};

// addSeries(3, "hello", [3,4,8,9,10])

</script>

<template>
  <header>
    <Header msg="都道府県別の総人口推移" />
  </header>

  <main>
    <PrefectureCheckbox @onAddSeries="addSeries" @onRemoveSeries="removeSeries" />
    <Chart
    :options="chartOptions"
    />
    <!-- <PopulationChart /> -->
  </main>
</template>

<style scoped>
main {
  margin: 0 auto;
  max-width: 1000px;
  padding: 0 20px;
}
</style>