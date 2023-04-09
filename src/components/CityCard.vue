<template>
  <div
    class="max-w-screen-md w-full p-4 rounded-md cursor-pointer bg-cover bg-gradient-to-r from-[#abc4ff] to-[#7eb6ff]"
  >
    <div class="flex flex-row justify-between text-weather-alert-primary mb-4">
      <p class="text-xl">
        {{ city.city }}, {{ city.state
        }}<span class="block text-xs"
          >Feels like
          {{ Math.round(city.weather.current.feels_like) }}&deg</span
        >
      </p>
      <p class="text-4xl font-bold">
        {{ Math.round(city.weather.current.temp) }}&deg
      </p>
    </div>
    <div
      class="flex flex-row justify-between items-end text-weather-alert-primary"
    >
      <p class="text-sm capitalize">
        {{ city.weather.current.weather[0].description }}
      </p>
      <p class="text-sm">
        <span class="pr-1"
          >H: {{ Math.round(city.weather.daily[0].temp.max) }}&deg</span
        >
        <span>L: {{ Math.round(city.weather.daily[0].temp.min) }}&deg</span>
      </p>
    </div>
  </div>
</template>

<script setup>
defineProps({
  city: {
    type: Object,
    default: () => {},
  },
});
</script>

<style lang="scss" scoped>
</style>