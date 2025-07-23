<script setup lang="ts">
import Widget from "../components/Widget.vue";
import ChartCard from "../components/ChartCard.vue";
import ChangeWidget from "../components/ChangeWidget.vue";
import BarChart from "../components/BarChart.vue";
import Description from "../components/Description.vue";

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

const formattedDownloads = computed(() => {
  if (overviewData?.value?.downloads)
    return overviewData.value.downloads / 1000;
  return 0;
});
const formattedRevenue = computed(() => {
  if (overviewData?.value?.revenue) return overviewData.value.revenue / 1000000;
  return 0;
});
</script>

<template>
  <div class="flex flex-col lg:flex-row">
    <div class="w-full lg:w-3/5">
      <div
        class="fontPoppins font-semibold text-2xl sm:text-3xl md:text-4xl text-black pb-4.5 pt-8 px-4 sm:px-6 md:px-9"
      >
        {{ overviewData?.name }}
      </div>
      <div
        class="flex flex-col sm:flex-row gap-4 sm:gap-[21px] mb-8 px-4 sm:px-6 md:px-8"
      >
        <Widget type="price" :value="`$${overviewData?.price.toString() || ''}`" />
        <Widget type="download" :value="`${formattedDownloads}k`" />
        <Widget type="revenue" :value="`${formattedRevenue}m`" />
      </div>
      <div class="bg-white rounded-3xl pt-7 px-4 sm:px-6 md:ml-8 md:mr-4">
        <ChartCard />
      </div>
    </div>
    <div
      class="w-full lg:w-[500px] bg-white rounded-3xl p-4 sm:p-6 flex flex-col mt-6 lg:mt-0 mx-4 lg:ml-8"
    >
      <Description />
      <div class="mt-6">
        <div class="flex items-center gap-2 mb-2">
          <div class="w-[2px] h-[20px] rounded-[1px] bg-black opacity-10"></div>
          <h3 class="text-black text-base font-semibold fontPoppins">
            Stream Performance
          </h3>
        </div>
        <div class="bg-[#F5F7FB] rounded-2xl">
          <BarChart />
        </div>
      </div>
      <div class="mt-6">
        <div class="flex items-center gap-2 mb-3">
          <div class="w-[2px] h-[20px] rounded-[1px] bg-black opacity-10"></div>
          <h3 class="text-black text-base font-semibold fontPoppins">
            Stream Stats
          </h3>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <ChangeWidget
            type="hoursWatched"
            :value="streamData?.hoursWatched.deltaPercentage?.toFixed(1) || ''"
            :delta="streamData?.hoursWatched.delta || 0"
            :delta-percentage="streamData?.hoursWatched.deltaPercentage || 0"
          />
          <ChangeWidget
            type="averageViewers"
            :value="streamData?.averageViewers.deltaPercentage?.toFixed(1) || ''"
            :delta="streamData?.averageViewers.delta || 0"
            :delta-percentage="streamData?.averageViewers.deltaPercentage || 0"
          />
        </div>
      </div>
    </div>
  </div>
</template>