<script setup lang="ts">
import { ref, onMounted } from "vue";
import {
  Chart,
  BarController,
  BarElement,
  CategoryScale,
  LinearScale,
  Title,
  Tooltip,
} from "chart.js";

const { data: streamData } = await useAsyncData<{
  hoursWatched: {
    value: number;
    delta: number;
    deltaPercentage: number;
  };
  averageViewers: {
    value: number;
    delta: number;
    deltaPercentage: number;
  };
  dailyStreamCounts: {
    Monday: number;
    Tuesday: number;
    Wednesday: number;
    Thursday: number;
    Friday: number;
  };
}>("stream", () =>
  $fetch("https://dhcase-mockapi.vercel.app/api/game/578080/stream")
);
Chart.register(
  BarController,
  BarElement,
  CategoryScale,
  LinearScale,
  Title,
  Tooltip
);

const canvasRef = ref<HTMLCanvasElement | null>(null);
const labels = Object.keys(streamData.value?.dailyStreamCounts || {});
const dataValues = Object.values(streamData.value?.dailyStreamCounts || {});

onMounted(() => {
  if (!canvasRef.value) return;

  const ctx = canvasRef.value.getContext("2d");
  if (!ctx) return;

  new Chart(ctx, {
    type: "bar",
    data: {
      labels: labels,
      datasets: [
        {
          label: "Stream Count",
          data: dataValues,
          backgroundColor: "#008DE4",
          borderRadius: 6,
          barPercentage: 1.5,
          categoryPercentage: 0.5,
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        title: {
          display: true,
          align: "start",
          color: "#000",
          font: {
            size: 16,
            weight: "bold",
            family: "Poppins",
          },
        },
        tooltip: {
          enabled: true,
        },
      },
      scales: {
        x: {
          ticks: {
            color: "#000",
            font: {
              size: 12,
              family: "Poppins",
            },
          },
          grid: {
            display: false,
          },
        },
        y: {
          beginAtZero: true,
          ticks: {
            stepSize: 1,
            color: "#000",
            font: {
              size: 12,
              family: "Poppins",
            },
          },
          grid: {
            color: "rgba(0, 0, 0, 0.05)",
          },
        },
      },
    },
  });
});
</script>

<template>
  <div class="w-full flex justify-center items-center">
    <div class="w-[300px] h-[200px] bg-[#F5F7FB] rounded-xl p-4">
      <canvas ref="canvasRef"></canvas>
    </div>
  </div>
</template>
