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

const chartCanvas = ref<HTMLCanvasElement | null>(null);

const labels = [
  "5",
  "6",
  "7",
  "8",
  "9",
  "10",
  "11",
  "12",
  "13",
  "14",
  "15",
  "16",
  "17",
  "18",
  "19",
  "20",
  "21",
  "22",
  "23",
  "0",
  "1",
  "2",
  "3",
  "4",
];
const data = {
  labels: labels,
  datasets: [
    {
      label: "My First Dataset",
      data: [
        0, 20, 40, 50, 60, 120, 150, 100, 50, 40, 20, 60, 120, 100, 50, 30, 20,
        10, 5, 0, 0, 0, 0, 0,
      ],
      fill: true,
      borderColor: "rgba(91, 46, 170, 1)",
      borderWidth: 2,
      tension: 0.4,
      pointRadius: 0,
    },
  ],
};

const options = {
  responsive: true,
  plugins: {
    legend: {
      display: false, // ラベルを非表示にする
    },
  },
  scales: {
    x: {
      beginAtZero: true,
      grid: {
        display: false,
      },
    },
    y: {
      beginAtZero: true,
      ticks: {
        stepSize: 50,
      },
    },
  },
};

onMounted(() => {
  if (chartCanvas.value) {
    const ctx = chartCanvas.value.getContext("2d");
    if (ctx) {
      // グラデーションの作成
      const gradient = ctx.createLinearGradient(
        0,
        0,
        0,
        chartCanvas.value.height * 3
      );
      gradient.addColorStop(0, "rgba(91, 46, 170, 0.6)");
      gradient.addColorStop(0.3, "rgba(91, 46, 170, 0.5)");
      gradient.addColorStop(0.5, "rgba(91, 46, 170, 0.3)");
      gradient.addColorStop(0.77, "rgba(91, 46, 170, 0.1)");
      gradient.addColorStop(0.9, "rgba(91, 46, 170, 0)");

      data.datasets[0].backgroundColor = gradient;

      new Chart(ctx, {
        type: "line",
        data: data,
        options: options,
      });
    }
  }
});
</script>

<style scoped>
.chart-container {
  max-width: 900px; /* 幅を調整 */
  max-height: 600px; /* 高さを調整 */
}
canvas {
  width: 100% !important;
  height: 100% !important;
}
</style>
