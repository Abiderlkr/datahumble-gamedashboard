<script setup lang="ts">
import sendLetter from "../assets/sendLetter.svg";
import sendLetterUp from "../assets/sendLetterUp.svg";

const props = defineProps<{
  type: "hoursWatched" | "averageViewers";
}>();

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
}>("stream", () =>
  $fetch("https://dhcase-mockapi.vercel.app/api/game/578080/stream")
);

const configs = {
  hoursWatched: {
    title: "Hours Watched",
    iconColor: "text-[#75F94C]",
    unit: "M",
  },
  averageViewers: {
    title: "Average Viewers",
    iconColor: "text-[#EB3223]",
    unit: "K",
  },
};

const data = computed(
  () =>
    streamData.value?.[props.type] || {
      value: 0,
      delta: 0,
      deltaPercentage: 0,
    }
);

const formattedMainValue = computed(() => {
  const value = data.value.value;
  return props.type === "hoursWatched"
    ? `${(value / 1000000).toFixed(1)}M`
    : `${(value / 1000).toFixed(1)}K`;
});

const formattedDelta = computed(() => {
  const delta = data.value.delta;
  const absDelta = Math.abs(delta);
  return props.type === "hoursWatched"
    ? `${(absDelta / 1000000).toFixed(1)}M`
    : `${absDelta.toLocaleString()}`;
});

const formattedPercentage = computed(() => {
  return `${Math.abs(data.value.deltaPercentage).toFixed(1)}%`;
});

const isPositive = computed(() => data.value.delta >= 0);
const arrowSvg = computed(() => {
  if (data.value.delta > 0) return sendLetter;
  return sendLetterUp;
});
</script>

<template>
  <div
    class="w-full sm:w-[220px] h-auto sm:h-[254px] rounded-2xl bg-[#F5F7FB] flex flex-col justify-between p-4"
  >
    <div class="font-semibold text-base sm:text-xl text-black fontPoppins">
      {{ configs[props.type].title }}
    </div>
    <div
      class="py-3 sm:py-4 text-3xl sm:text-5xl font-semibold text-black fontPoppins"
    >
      {{ formattedMainValue }}
    </div>
    <div class="flex items-center gap-1 fontPoppins text-base sm:text-xl">
      <img :src="arrowSvg" class="w-5 h-5 sm:w-6 sm:h-6" />
      <span
        :class="[isPositive ? 'text-[#75F94C]' : 'text-[#EB3223]']"
        class="font-semibold"
      >
        {{ isPositive ? "" : "-" }}{{ formattedDelta }}
      </span>
      <span class="text-black">({{ formattedPercentage }})</span>
    </div>
  </div>
</template>
