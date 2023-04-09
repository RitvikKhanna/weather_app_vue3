<template>
  <div class="flex flex-col flex-1 items-center cursor-default">
    <!-- Banner -->
    <div
      v-if="route.query.preview"
      class="text-weather-secondary p-6 bg-weather-alert-primary w-full text-center"
    >
      <p class="text-white">
        You are in preview mode. Click the "+" icon to add it to your list.
      </p>
    </div>

    <!-- Preview -->
    <div
      class="w-full flex flex-col items-center text-weather-secondary pt-12 pb-8"
    >
      <h1 class="text-5xl mb-2">
        {{ route.params.city }}
      </h1>
      <p class="text-sm mb-12 mt-2">
        {{
          new Date(weatherData.currentTime).toLocaleDateString("en-us", {
            weekday: "long",
            day: "numeric",
            month: "long",
          })
        }}
        --
        {{
          new Date(weatherData.currentTime).toLocaleTimeString("en-us", {
            hour: "2-digit",
            minute: "2-digit",
          })
        }}
      </p>
      <p class="text-8xl mb-1 flex items-center">
        <img
          class="w-[150px] h-auto"
          :src="`https://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`"
          alt="Image Not Found"
        />
        {{ Math.round(weatherData.current.temp) }}&deg
      </p>
      <p class="mb-1 text-center">
        Feels like {{ Math.round(weatherData.current.feels_like) }} &deg
      </p>
      <p class="capitalize mb-12">
        {{ weatherData.current.weather[0].description }}
      </p>
      <hr
        class="border-opacity-10 border-weather-alert-primary border w-full"
      />

      <!-- Hourly -->
      <div class="max-w-screen-md w-full py-12">
        <div class="mx-8 text-weather-secondary">
          <h1 class="mb-8 font-bold text-lg">Hourly Weather</h1>
          <div class="flex gap-10 overflow-x-scroll">
            <div
              class="flex flex-col gap-2 items-center"
              v-for="hourData in weatherData.hourly"
              :key="hourData.dt"
            >
              <p class="whitespace-nowrap text-md">
                {{
                  new Date(hourData.currentTime).toLocaleTimeString("en-us", {
                    hour: "numeric",
                  })
                }}
              </p>
              <img
                class="w-auto h-[50px] object-cover"
                :src="`https://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`"
                alt="Image Not Found"
              />
              <p class="text-xl mb-10">{{ Math.round(hourData.temp) }}&deg</p>
            </div>
          </div>
        </div>
      </div>
      <hr
        class="border-opacity-10 border-weather-alert-primary border w-full"
      />

      <!-- Daily -->
      <div class="max-w-screen-md w-full pt-12 pb-1">
        <div class="mx-8 text-weather-secondary">
          <h1 class="mb-8 font-bold text-lg">7 Day Forecast</h1>
          <div
            class="flex items-center"
            v-for="dailyData in weatherData.daily"
            :key="dailyData.dt"
          >
            <p class="flex-1 my-2">
              {{
                new Date(dailyData.dt * 1000).toLocaleDateString("en-us", {
                  weekday: "long",
                })
              }}
            </p>
            <img
              class="w-[50px] h-[50px] object-cover"
              :src="`https://openweathermap.org/img/wn/${dailyData.weather[0].icon}@2x.png`"
              alt="Image Not Found"
            />
            <div class="flex gap-2 flex-1 justify-end">
              <p class="mr-2">
                <i class="fa-solid fa-temperature-arrow-up mr-1"></i
                >{{ Math.round(dailyData.temp.max) }}&deg
              </p>
              <p>
                <i class="fa-solid fa-temperature-arrow-down mr-2"></i
                >{{ Math.round(dailyData.temp.min) }}&deg
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Delete City from storage -->
    <div
      @click="removeCity"
      class="mt-1 mb-10 flex justify-center items-center text-weather-primary cursor-pointer bg-weather-secondary py-4 px-11 rounded-md text-xl hover:text-[#dd2d4a]"
      v-if="!route.query.preview"
    >
      Remove City
      <i class="fa-solid fa-trash-can ml-2"></i>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();
const openWeatherAPIKey = "5e4c40c7c172d2e45f31aa4097ac62f1";

const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&&exclude={part}&appid=${openWeatherAPIKey}&units=metric`
    );

    // Calculate current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    // Calculate hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
    });
    return weatherData.data;
  } catch {
    console.log(error);
  }
};

await new Promise((res)=> setTimeout(res,1000))

const removeCity = () => {
  let cities = [];
  if (localStorage.getItem("savedCities")) {
    cities = JSON.parse(localStorage.getItem("savedCities"));
    cities = cities.filter((city) => city.city !== route.params.city);
    localStorage.setItem("savedCities", JSON.stringify(cities));

    router.push({
    name: "cityView",
    params: {
      state: route.params.state,
      city: route.params.city,
    },
    query: {
      lat: route.query.lat,
      lng: route.query.lng,
      preview: true,
    },
  });
  }
};

const weatherData = await getWeatherData();
</script>

