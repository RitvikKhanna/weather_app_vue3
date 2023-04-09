<template>
  <main class="container text-weather-secondary">
    <div class="pt-4 mb-8 relative">
      <input
        @input="searchResults"
        v-model="searchQuery"
        type="text"
        class="py-2 text-weather-alert-primary px-1 w-full bg-transparent border-weather-alert-primary border-b-2 focus:border-weather-alert-primary placeholder-weather-alert-primary focus:outline-none focus-sha focus:shadow-[0px_1px_0_0#004e71]"
        placeholder="Search for a city or a state..."
      />
      <ul
        v-if="mapSearchResults"
        class="absolute bg-weather-alert-primary text-white w-full shadow-md top-[66px]"
      >
        <template v-if="searchError">
          <p class="py-2 font-bold text-weather-alert">Sorry, something went wrong, please try again.</p>
        </template>

        <template v-else-if="!searchError && mapSearchResults.length === 0">
          <p class="p-3 cursor-default text-weather-alert">No results match your query, try a different term.</p>
        </template>

        <template v-else>
          <li
            v-for="searchResult in mapSearchResults"
            :key="searchResult.id"
            class="cursor-pointer px-2 text-white py-3 hover:bg-weather-hover"
            @click="previewCity(searchResult)"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <CityCardSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import {useRouter} from "vue-router"
import CityList from "../components/CityList.vue";
import CityCardSkeleton from "../components/CityCardSkeleton.vue"

const router = useRouter();
const mapBoxAPIKey =
  "pk.eyJ1IjoicmtoYW5uYSIsImEiOiJjbGc1d3FweGMwN3EwM2dvOGJmYnBvenJnIn0.Fr-AHgfYUt68-Ywx0KPeWg";
const searchQuery = ref("");
const mapSearchResults = ref(null);
const searchError = ref(false);

async function searchResults() {
  if (searchQuery.value !== "") {
    try {
      const response = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapBoxAPIKey}&types=place`
      );
      mapSearchResults.value = response.data.features;
      return;
    } catch {
      searchError.value = true;
    }
  }
  mapSearchResults.value = null;
}

const previewCity = (searchResult) => {
  const [city,state,country] = searchResult.place_name.split(',')

  // <url>/weather/state/city?lat=111&lng=000&preview=true
  router.push({
    name: 'cityView',
    // Params go right after the url
    params: {
      state: state.replaceAll(" ",""),
      city: city.replaceAll(" ",""),
    },
    // Query goes after the params
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    }
  })
}

</script>