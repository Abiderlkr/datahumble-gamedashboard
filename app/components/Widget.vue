<script setup lang="ts">
import ticket from '../assets/ticket.svg'
import robo from '../assets/robo.svg'
import cash from '../assets/cash.svg'

const props = defineProps<{
type:'price' | 'download' | 'revenue',
value: string
}>()

const configs:Record<'price' | 'download' | 'revenue',any> = {
    price : {
        svg: ticket,
        bg: 'bg-[#73B0FF66]',
        title: 'Price',

    },
    download : {
        svg: robo,
        bg: 'bg-[#FF73E166]',
        title: 'Downloads',

    },
       revenue : {
        svg: cash,
        bg: 'bg-[#FF737666]',
        title: 'Revenue',

    }
}
</script>

<template>
  <div class="w-[342px] h-[156px] rounded-3xl bg-white px-5.5 py-5.5 flex items-center gap-6 shadow-sm">
    <div class="w-[112px] h-[112px] rounded-xl flex items-center justify-center" :class="`${configs[props.type].bg}`">
      <img :src="configs[props.type].svg" class="w-10 h-10" />
    </div>
    <div class="flex flex-col justify-center fontPoppins">
      <span class="text-2xl font-semibold text-black">{{ props.value }}</span>
      <span class="text-sm text-black font-medium  mt-[46px]">{{configs[props.type].title}}</span>
    </div>
  </div>
</template>
