<script setup lang="ts">
import sidebarHeader from "../assets/sidebarHeader.svg";
import dashboardIcon from "../assets/dashboardIcon.svg";
import inboxIcon from "../assets/inboxIcon.svg";
import settingsIcon from "../assets/settingsIcon.svg";
import logoutIcon from "../assets/logoutIcon.svg";
</script>

<template>
  <div
    class="w-[212px] h-dvh bg-white rounded-lg flex flex-col justify-between py-4 px-4 shadow-sm"
  >
    <div>
      <div class="flex justify-center pb-6">
        <img :src="sidebarHeader" />
      </div>
      <div class="mt-10">
        <span class="text-xs text-[#777777] fontPoppins px-2 mb-2 block"
          >OVERVIEW</span
        >
        <div
          class="flex items-center gap-3 bg-[#F5F7FB] rounded-lg px-3 py-2 mt-1.5"
        >
          <img :src="dashboardIcon" class="w-5 h-5" />
          <span class="text-[#111111] font-medium fontRoboto text-sm"
            >Dashboard</span
          >
        </div>
        <div
          class="flex items-center gap-3 rounded-lg px-3 py-2 mt-2 hover:bg-[#F5F7FB] cursor-pointer"
        >
          <img :src="inboxIcon" class="w-5 h-5" />
          <span class="text-[#777777] text-sm fontRoboto font-medium"
            >Inbox</span
          >
        </div>
      </div>
    </div>
    <div class="mb-4">
      <span class="text-xs text-[#777777] fontPoppins px-2 block mb-3"
        >SETTINGS</span
      >

      <div
        class="flex items-center gap-3 rounded-md px-3 py-2 mb-2 hover:bg-[#F5F7FB] cursor-pointer"
      >
        <img :src="settingsIcon" class="w-5 h-5" />
        <span class="text-[#777777] text-sm fontRoboto font-medium"
          >Settings</span
        >
      </div>

      <div
        class="flex items-center gap-3 rounded-md px-3 py-2 hover:bg-red-50 cursor-pointer"
      >
        <img :src="logoutIcon" class="w-5 h-5" />
        <span class="text-sm fontRoboto font-medium text-[#FC2220]"
          >Logout</span
        >
      </div>
    </div>
  </div>
</template>
