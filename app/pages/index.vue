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
  <div class="flex">
    <div>
      <div
        class="fontPoppins font-semibold text-4xl text-black pb-4.5 pt-7 pl-9">{{ overviewData?.name }}</div>
      <div class="flex gap-[21px] mb-8 ml-8">
        <Widget type="price" :value="`$${overviewData?.price.toString() || ''}`"/>
        <Widget type="download" :value="`${formattedDownloads}k`" />
        <Widget type="revenue" :value="`${formattedRevenue}m`" />
      </div>
      <div class="bg-white rounded-3xl pt-7 ml-8"><ChartCard /></div>
    </div>
    <div class="w-[500px] bg-white rounded-3xl p-6 flex flex-col ml-8">
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
        <div class="grid grid-cols-2 gap-4">
          <ChangeWidget
            type="hoursWatched"
            :value="streamData?.hoursWatched.deltaPercentage?.toFixed(1) || ''"
            :delta="streamData?.hoursWatched.delta || 0"
            :delta-percentage="streamData?.hoursWatched.deltaPercentage || 0"
          />
          <ChangeWidget
            type="averageViewers"
            :value="
              streamData?.averageViewers.deltaPercentage?.toFixed(1) || ''
            "
            :delta="streamData?.averageViewers.delta || 0"
            :delta-percentage="streamData?.averageViewers.deltaPercentage || 0"
          />
        </div>
      </div>
    </div>
  </div>
</template>
