<template>
  <div
    class="w-full flex flex-col items-center text-weather-secondary pt-12 pb-8"
  >
    <AnimatedPlaceHolder class="max-w-[250px] h-[50px] w-full mb-2" />
    <AnimatedPlaceHolder class="max-w-[300px] w-full mb-24" />
    <AnimatedPlaceHolder class="max-w-[250px] h-[70px] w-full mb-10" />
    <AnimatedPlaceHolder class="max-w-[100px] w-full mb-2" />
    <AnimatedPlaceHolder class="max-w-[70px] w-full mb-12" />
    <hr
      class="border-opacity-10 border-weather-alert-primary border w-full mb-12"
    />
    <div class="flex flex-col px-12 w-full">
      <AnimatedPlaceHolder class="max-w-[120px] w-full mb-8 self-start" />
      <AnimatedPlaceHolder class="w-full h-[140px] mb-12" />
    </div>
    <hr
      class="border-opacity-10 border-weather-alert-primary border w-full mb-12"
    />
    <div class="flex flex-col px-12 w-full">
      <AnimatedPlaceHolder class="max-w-[150px] w-full mb-8 self-start" />
      <AnimatedPlaceHolder class="w-full h-[250px] mb-12" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceHolder from "./AnimatedPlaceHolder.vue";
</script>

// <AnimatedPlaceHolder class="max-w-[150px] w-full" />