<script setup lang="ts">
import Sidebar from '../components/Sidebar.vue';
import NavHeader from '../components/NavHeader.vue';
</script>
<template>
  <div class="flex w-screen max-w-screen overflow-hidden bg-[#F5F7FB]">
    <div class="px-4 py-4"><Sidebar/></div>
    <div class="flex-1">
      <div class="pt-4 pl-8 pb-8"><NavHeader/></div>
      <slot />
    </div>
  </div>
</template>
