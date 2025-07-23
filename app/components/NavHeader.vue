<script setup lang="ts">
import searchIcon from "../assets/search.svg";
import notificationIcon from "../assets/notificationsIcon.svg";
import mailIcon from "../assets/mailIcon.svg";
import russell from "../assets/russell.svg";
</script>

<template>
  <div
    class="w-full flex justify-between items-center bg-[#F5F7FB] p-4 rounded-xl"
  >
    <div
      class="w-[700px] flex items-center bg-white rounded-2xl px-4 py-4 shadow-sm"
    >
      <img :src="searchIcon" class="w-5 h-5" />
      <input
        type="text"
        placeholder="Search..."
        class="ml-3 w-full outline-none text-sm text-[#999999] fontPoppins bg-transparent placeholder:text-[#999999]"
      />
    </div>
    <div class="flex items-center gap-4">
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
      <div class="w-px h-6 bg-gray-300"></div>
      <div class="flex items-center gap-2">
        <img :src="russell" class="w-12 h-12" />
        <span class="fontRoboto text-xl font-medium px-8 pl-2.5 text-black">Russell</span>
      </div>
    </div>
  </div>
</template>
