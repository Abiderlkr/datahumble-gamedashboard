<script setup lang="ts">
import { ref, computed, onMounted } from "vue";
import {
  Chart,
  LineController,
  LineElement,
  PointElement,
  LinearScale,
  Title,
  Tooltip,
  Filler,
  CategoryScale,
} from "chart.js";
import { Menu, MenuButton, MenuItems, MenuItem } from "@headlessui/vue";

const { data: performanceData } = await useAsyncData<{
  dates: string[];
  watchTime: number[];
  engagement: number[];
}>("performance", () =>
  $fetch("https://dhcase-mockapi.vercel.app/api/game/578080/performance")
);

const { data: overviewData } = await useAsyncData<{
  gameId: number;
  name: string;
  price: number;
  downloads: number;
  revenue: number;
  description: string;
}>("overview", () =>
  $fetch("https://dhcase-mockapi.vercel.app/api/game/578080/overview")
);

Chart.register(
  LineController,
  LineElement,
  PointElement,
  LinearScale,
  Title,
  Tooltip,
  Filler,
  CategoryScale
);

const canvasRef = ref<HTMLCanvasElement | null>(null);

const formatDate = (dateStr: string) => {
  const date = new Date(dateStr);
  const day = String(date.getDate()).padStart(2, "0");
  const month = date.toLocaleString("en-US", { month: "short" });
  return `${day} ${month}`;
};

const selectedDate = ref("");

const dateOptions = computed(() => {
  const dates = performanceData.value?.dates;
  if (!dates || dates.length === 0) return [];

  const first = formatDate(dates[0]!);
  const last = formatDate(dates[dates.length - 1]!);
  const range = `${first} - ${last}`;

  return [range];
});

watchEffect(() => {
  const firstOption = dateOptions.value?.[0];
  if (!selectedDate.value && firstOption) {
    selectedDate.value = firstOption;
  }
});

onMounted(() => {
  if (!canvasRef.value) return;

  const ctx = canvasRef.value.getContext("2d");
  if (!ctx || !performanceData.value) return;

  new Chart(ctx, {
    type: "line",
    data: {
      labels: performanceData.value.dates.map((x) => x.split("-").at(-1)),
      datasets: [
        {
          label: "Watch Time",
          data: performanceData.value.watchTime,
          fill: true,
          borderColor: "#4A90E2",
          backgroundColor: "rgba(74, 144, 226, 0.2)",
          tension: 1,
          pointRadius: 0,
        },
        {
          label: "Engagement",
          data: performanceData.value.engagement,
          fill: true,
          borderColor: "#F5A623",
          backgroundColor: "rgba(245, 166, 35, 0.2)",
          tension: 1,
          pointRadius: 0,
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          mode: "index",
          intersect: false,
        },
      },
      scales: {
        y: {
          beginAtZero: true,
          ticks: {
            callback: (val) => `${val}h`,
          },
          grid: {
            color: "rgba(0,0,0,0.05)",
          },
        },
        x: {
          grid: {
            display: false,
          },
        },
      },
    },
  });
});

const handleSelect = (option: string) => {
  selectedDate.value = option;
};
</script>

<template>
  <div class="w-full max-w-[1200px] mx-auto p-4 sm:p-6 md:p-8">
    <div class="flex items-center justify-between mb-4">
      <h2 class="text-xl font-semibold text-black fontPoppins">Performance</h2>
      <Menu as="div" class="relative inline-block text-left">
        <MenuButton
          class="bg-[#F5F7FB] text-[#777777] text-sm fontPoppins rounded-md px-3 py-1 min-w-[115px]"
        >
          {{ selectedDate }}
        </MenuButton>
        <MenuItems
          class="absolute right-0 mt-2 w-full bg-white rounded-md shadow-lg text-sm fontPoppins z-10"
        >
          <MenuItem
            v-for="option in dateOptions"
            :key="option"
            v-slot="{ active }"
          >
            <button
              @click="handleSelect(option)"
              :class="[
                'block w-full text-left px-3 py-2',
                active ? 'bg-[#F5F7FB]' : '',
              ]"
            >
              {{ option }}
            </button>
          </MenuItem>
        </MenuItems>
      </Menu>
    </div>
    <div class="w-full h-[245px]">
      <canvas ref="canvasRef"></canvas>
    </div>
    <div class="mt-6">
      <h3 class="text-xl font-semibold text-black fontPoppins mb-3">
        Description
      </h3>
      <div class="bg-[#F5F7FB] rounded-xl w-full p-4">
        <span
          class="text-[#777777] text-sm font-medium fontPoppins leading-relaxed"
        >
          {{ overviewData?.description }}
        </span>
      </div>
    </div>
  </div>
</template>
