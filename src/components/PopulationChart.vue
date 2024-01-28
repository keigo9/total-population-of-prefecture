<script setup>
import VueHighcharts from 'vue3-highcharts';
import { ref, computed } from 'vue';

const seriesData = ref([{}]);
const categories = ref([ 
  1965, 1970, 1975, 1980, 1985, 1990, 1995, 2000, 2005, 2010, 
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
  },
  yAxis: {
    title: {
      text: '人口数',
    },
  },
  series: seriesData.value,
}));

const addSeries = (id, name, population) => {
  seriesData.value.push({
    id,
    name,
    data: population,
  });
};

const removeSeries = (id) => {
  seriesData.value = seriesData.value.filter(val => val.id !== id);
};
</script>

<template>
  <vue-highcharts
    type="chart"
    :options="chartOptions"
    :redrawOnUpdate="true"
    :oneToOneUpdate="true"
    :animateOnUpdate="true"
    @updated="onUpdated"/>
</template>


