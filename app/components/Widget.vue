<script setup lang="ts">
import ticket from "../assets/ticket.svg";
import robo from "../assets/robo.svg";
import cash from "../assets/cash.svg";

const props = defineProps<{
  type: "price" | "download" | "revenue";
  value: string;
}>();

const configs: Record<"price" | "download" | "revenue", any> = {
  price: {
    svg: ticket,
    bg: "bg-[#73B0FF66]",
    title: "Price",
  },
  download: {
    svg: robo,
    bg: "bg-[#FF73E166]",
    title: "Downloads",
  },
  revenue: {
    svg: cash,
    bg: "bg-[#FF737666]",
    title: "Revenue",
  },
};
</script>

<template>
  <div
    class="w-full sm:w-auto h-auto sm:h-[156px] rounded-3xl bg-white px-4 sm:px-5.5 py-4 sm:py-5.5 flex items-center gap-4 sm:gap-6 shadow-sm"
  >
    <div
      class="w-[80px] h-[80px] sm:w-[112px] sm:h-[112px] rounded-xl flex items-center justify-center"
      :class="configs[props.type].bg"
    >
      <img :src="configs[props.type].svg" class="w-8 h-8 sm:w-10 sm:h-10" />
    </div>
    <div class="flex flex-col justify-center fontPoppins">
      <span class="text-xl sm:text-2xl font-semibold text-black">{{
        props.value
      }}</span>
      <span class="text-xs sm:text-sm text-black font-medium mt-3 sm:mt-[46px]">
        {{ configs[props.type].title }}
      </span>
    </div>
  </div>
</template>
