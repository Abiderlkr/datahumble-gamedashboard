<script setup lang="ts">
import searchIcon from "../assets/search.svg";
import notificationIcon from "../assets/notificationsIcon.svg";
import mailIcon from "../assets/mailIcon.svg";
import russell from "../assets/russell.svg";
</script>

<template>
  <div
    class="w-full flex flex-col sm:flex-row sm:justify-between sm:items-center bg-[#F5F7FB] p-4 rounded-xl gap-4 sm:gap-0"
  >
    <div
      class="w-full sm:w-[700px] flex items-center bg-white rounded-2xl px-4 py-3 shadow-sm"
    >
      <img :src="searchIcon" class="w-5 h-5" />
      <input
        type="text"
        placeholder="Search..."
        class="ml-3 w-full outline-none text-sm text-[#999999] fontPoppins bg-transparent placeholder:text-[#999999]"
      />
    </div>
    <div class="flex flex-wrap sm:flex-nowrap items-center gap-3 sm:gap-4">
      <div
        class="w-10 h-10 rounded-xl bg-white flex items-center justify-center"
      >
        <img :src="mailIcon" class="w-5 h-5" />
      </div>
      <div
        class="relative w-10 h-10 rounded-xl bg-white flex items-center justify-center"
      >
        <img :src="notificationIcon" class="w-5 h-5" />
      </div>
      <div class="hidden sm:block w-px h-6 bg-gray-300"></div>
      <div class="flex items-center gap-2">
        <img :src="russell" class="w-10 h-10 sm:w-12 sm:h-12 rounded-full" />
        <span class="fontRoboto text-base sm:text-xl font-medium text-black">
          Russell
        </span>
      </div>
    </div>
  </div>
</template>
