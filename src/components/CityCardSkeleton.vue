<template>
  <div class="max-w-screen-md w-full p-4 rounded-md">
    <div class="flex justify-between">
      <AnimatedPlaceHolder class="max-w-[150px] w-full" />
      <AnimatedPlaceHolder class="max-w-[70px] w-full" />
    </div>
    <AnimatedPlaceHolder class="mt-2 max-w-[110px] w-full" />
    <div class="mt-4 flex justify-between">
      <AnimatedPlaceHolder class="max-w-[70px] w-full" />
      <AnimatedPlaceHolder class="max-w-[100px] w-full" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceHolder from "./AnimatedPlaceHolder.vue";
</script>