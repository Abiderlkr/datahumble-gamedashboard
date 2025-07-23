<script setup lang="ts">
import { ref } from "vue";
import Sidebar from "../components/Sidebar.vue";
import NavHeader from "../components/NavHeader.vue";

const isSidebarOpen = ref(false);
</script>

<template>
  <div
    class="flex flex-col lg:flex-row w-screen h-dvh max-w-screen overflow-hidden bg-[#F5F7FB]"
  >
    <div class="lg:hidden p-4">
      <button
        @click="isSidebarOpen = !isSidebarOpen"
        class="text-black text-xl"
      >
        ☰
      </button>
    </div>
    <div class="hidden lg:block px-4 py-4">
      <Sidebar />
    </div>
    <div
      v-if="isSidebarOpen"
      class="fixed inset-0 z-50 bg-white px-4 py-4 shadow-xl lg:hidden"
    >
      <div class="flex justify-end mb-2">
        <button @click="isSidebarOpen = false" class="text-xl">✕</button>
      </div>
      <Sidebar />
    </div>
    <div class="flex-1 flex flex-col">
      <div class="pt-4 px-4 sm:px-6 md:px-8">
        <NavHeader />
      </div>
      <div class="flex-1 overflow-y-auto px-4 sm:px-6 md:px-8 pb-8">
        <slot />
      </div>
    </div>
  </div>
</template>
