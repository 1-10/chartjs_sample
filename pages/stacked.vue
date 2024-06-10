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

const labels = ["5/13", "5/14", "5/15", "5/16", "5/17", "5/18", "5/19"];

const datasets = [
  {
    label: "回答表示",
    data: [200, 150, 180, 160, 140, 120, 130],
    backgroundColor: "rgba(91, 46, 170)",
    borderColor: "rgba(91, 46, 170, 1)",
    borderWidth: 1,
    borderRadius: 5, // 角丸の半径
  },
  {
    label: "候補表示",
    data: [20, 15, 30, 25, 20, 10, 20],
    backgroundColor: "rgba(214, 107, 201)",
    borderColor: "rgba(214, 107, 201, 1)",
    borderWidth: 1,
  },
  {
    label: "候補なし",
    data: [5, 5, 10, 8, 5, 3, 5],
    backgroundColor: "rgba(165, 131, 196)",
    borderColor: "rgba(165, 131, 196, 1)",
    borderWidth: 1,
  },
];

// データセットを並べ替え（データの和でソート）
datasets.sort((a, b) => {
  const sumA = a.data.reduce((acc, val) => acc + val, 0);
  const sumB = b.data.reduce((acc, val) => acc + val, 0);
  return sumA - sumB; // 昇順に並べ替え
});

const data = {
  labels: labels,
  datasets: datasets,
};

const options = {
  responsive: true,
  scales: {
    x: {
      stacked: true,
      grid: {
        display: false,
      },
    },
    y: {
      stacked: true,
      beginAtZero: true,
    },
  },
};

onMounted(() => {
  if (chartCanvas.value) {
    const ctx = chartCanvas.value.getContext("2d");
    if (ctx) {
      new Chart(ctx, {
        type: "bar",
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
