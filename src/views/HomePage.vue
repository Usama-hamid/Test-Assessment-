<template>
  <v-container>
    <div class="d-flex justify-space-between">
      <span>Information: {{ MetaData["1. Information"] }}</span>
      <span>Last Refreshed: {{ MetaData["3. Last Refreshed"] }}</span>
      <span>Time Zone: {{ MetaData["5. Time Zone"] }}</span>
    </div>
    <div v-if="items" class="wrapper">
      <v-card
        v-for="(row, i) in TimeSeriesData"
        :key="i"
        class="ChartBox"
        elevation="4"
      >
        <!-- <div class="dummyBox">asdasdasd</div> -->
        <ApexChart :label="i" :chartData="row" />
      </v-card>
    </div>
  </v-container>
</template>

<script setup>
import ApexChart from "@/components/ApexChart.vue";
import { computed, onMounted, reactive } from "vue";

const items = reactive({});
const MetaData = computed(() => {
  return items.value ? items.value["Meta Data"] : {};
});
const TimeSeriesData = computed(() => {
  return items.value ? items.value["Time Series (Daily)"] : [];
});
onMounted(() => {
  fetchData();
});

async function fetchData() {
  const URL =
    "https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=IBM&apikey=demo";
  const res = await fetch(URL);
  const Data = await res.json();
  items.value = Data;
  console.log("data:", items.value);
}
</script>
<style scoped>
.dummyBox {
  background: red;
  height: 250px;
}
.wrapper {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 1rem;
}
.ChartBox {
  flex-basis: 350px;
}
</style>
