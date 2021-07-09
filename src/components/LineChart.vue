<template>
  <canvas id="chart-canvas"></canvas>
</template>

<script>
import Chart from "chart.js";

export default {
  name: "subscribtion-chart",
  props: {
    labels: Array,

    datasets: {
      type: Array,
      required: true,
    },

    options: Object,
  },
  data() {
    return {
      chart: null,
    };
  },
  watch: {
    datasets(newDatasets) {
      this.chart.data.datasets = newDatasets;
      this.chart.update();
    },
    labels(newLabels) {
      this.chart.data.labels = newLabels;
      this.chart.update();
    },
  },
  mounted() {
    this.chart = new Chart(document.getElementById("chart-canvas"), {
      type: "line",
      data: {
        labels: this.labels,
        datasets: this.datasets,
      },
      options: this.options,
    });
  },
  beforeUnmount() {
    if (this.chart) {
      this.chart.destroy();
    }
  },
};
</script>