<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard @click="goToCityView(city)" :city="city" />
  </div>
  <p v-if="savedCities.length === 0">
    You have no saved cities. Search a location in the field above and add it.
  </p>
</template>

<script setup>
import CityCard from "./CityCard.vue";
import { ref } from "vue";
import { useRouter } from "vue-router";

const savedCities = ref([]);
const router = useRouter();

await new Promise((res)=> setTimeout(res,1000))

const getCities = async () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));

    savedCities.value = savedCities.value.filter((city, index, self) => {
      return self.findIndex((v) => v.city === city.city) === index;
    });

    const requests = [];
    savedCities.value.forEach((city) => {
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/onecall?lat=${city.coords.lat}&lon=${city.coords.lng}&&exclude={part}&appid=5e4c40c7c172d2e45f31aa4097ac62f1&units=metric`
        )
      );
    });

    const weatherData = await Promise.all(requests);
    weatherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data;
    });
  }
};

await getCities();

const goToCityView = (city) => {
  router.push({
    name: "cityView",
    params: {
      state: city.state,
      city: city.city,
    },
    query: {
      lat: city.coords.lat,
      lng: city.coords.lng,
    },
  });
};
</script>