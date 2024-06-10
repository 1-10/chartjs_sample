<template>
  <Header />
  <div class="mt-4 ml-4">
    <div class="chart-container">
      <canvas ref="chartCanvas"></canvas>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { Chart } from "chart.js/auto";
import ChartDataLabels from "chartjs-plugin-datalabels";

const chartCanvas = ref<HTMLCanvasElement | null>(null);

const data = {
  labels: ["回答表示", "候補表示", "候補なし"],
  datasets: [
    {
      data: [90.2, 7.6, 2.2],
      backgroundColor: [
        "rgba(91, 46, 170)",
        "rgba(214, 107, 201)",
        "rgba(165, 131, 196)",
      ],
      borderColor: [
        "rgba(91, 46, 170, 1)",
        "rgba(214, 107, 201, 1)",
        "rgba(165, 131, 196, 1)",
      ],
      borderWidth: 1,
    },
  ],
};

// 最大の値を計算
const maxValue = Math.max(...data.datasets[0].data);

const options = {
  responsive: true,
  plugins: {
    legend: {
      display: true,
      position: "top",
    },
    tooltip: {
      callbacks: {
        label: function (context) {
          const label = context.label || "";
          const value = context.raw || 0;
          return `${label}: ${value}%`;
        },
      },
    },
    datalabels: {
      color: "white",
      formatter: (value) => {
        // 最大の値のみラベルを表示
        if (value === maxValue) {
          return `${value}%`;
        }
        return "";
      },
      font: {
        weight: "bold",
        size: 50,
      },
    },
  },
};

onMounted(() => {
  if (chartCanvas.value) {
    const ctx = chartCanvas.value.getContext("2d");
    if (ctx) {
      Chart.register(ChartDataLabels);
      new Chart(ctx, {
        type: "pie",
        data: data,
        options: options,
      });
    }
  }
});
</script>

<style scoped>
.chart-container {
  max-width: 600px; /* 幅を調整 */
  max-height: 600px; /* 高さを調整 */
}
canvas {
  width: 100% !important;
  height: 100% !important;
}
</style>
