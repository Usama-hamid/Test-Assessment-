<template>
  <div id="chart">
    <apexchart
      type="bar"
      height="350"
      :options="chartOptions"
      :series="series"
    ></apexchart>
  </div>
</template>

<script>
export default {
  props: ["label", "chartData"],
  data() {
    return {
      series: [],

      chartOptions: {
        chart: {
          type: "bar",
          height: 200,
        },
        plotOptions: {
          bar: {
            borderRadius: 4,
            horizontal: true,
          },
        },
        dataLabels: {
          enabled: false,
        },
        xaxis: {
          categories: [],
        },
      },
    };
  },

  beforeMount() {
    // for (const item of items) {}
    let labels = [];
    let values = [];
    for (let key in this.chartData) {
      labels.push(key);
      values.push(this.chartData[key]);
    }
    this.chartOptions.xaxis.categories = labels;
    this.series.push({ data: values });
  },
};
</script>
