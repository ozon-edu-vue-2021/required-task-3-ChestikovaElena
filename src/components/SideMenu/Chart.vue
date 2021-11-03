<template>
  <Doughnut ref="chart" />
</template>

<script>
import { Doughnut } from "vue-chartjs";
import getEmptyArray from "@/utils/consts.js";

export default {
  name: "Chart",
  props: {
    tables: {
      type: Array,
      default: getEmptyArray,
    },
    legend: {
      type: Array,
      default: getEmptyArray,
    },
  },
  components: {
    Doughnut,
  },
  mounted() {
    this.makeChart();
  },
  methods: {
    makeChart() {
      const chartData = {
        labels: this.legend.map(({ text }) => text),
        datasets: [
          {
            label: "Легенда",
            backgroundColor: this.legend.map(({ color }) => color),
            data: this.legend.map(({ counter }) => counter),
          }
        ],
      };
      const options = {
        borderWidth: "10px",
        legend: {
          display: false,
        }
      };

      this.$refs.chart.renderChart(chartData, options);
    },
  }
}
</script>

<style>

</style>