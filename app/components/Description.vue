<script setup lang="ts">
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
</script>

<template>
  <div class="flex flex-col gap-2 px-4 sm:px-0">
    <div class="flex items-center gap-2">
      <div class="w-[2px] h-[20px] rounded-[1px] bg-black opacity-10"></div>
      <h3 class="text-black text-sm sm:text-base font-semibold fontPoppins">
        Short Description
      </h3>
    </div>
    <span
      class="text-xs sm:text-sm text-[#777777] font-medium fontRoboto leading-relaxed"
    >
      {{ overviewData?.description }}
    </span>
  </div>
</template>
