<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        v-model="searchQuery"
        @input="getSearchResults"
        type="text"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      <ul
        v-if="mapboxSearchResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      >
        <p v-if="searchError">Sorry, something went wrong, please try again</p>
        <p v-if="!searchError && mapboxSearchResults.length === 0">
          No results match query, try a different term.
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
            @click="previewCity(searchResult)"
          >
            <!-- V5 -->
            <!-- {{ searchResult.place_name }} -->
            <!-- V6 -->
            {{ searchResult.properties.full_address }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();

// V5
// const previewCity = (searchResult) => {
//   console.log(searchResult);
//   const [city, state] = searchResult.place_name.split(", ");
//   router.push({
//     name: "cityView",
//     params: {
//       state: state.replaceAll(" ", ""),
//       city: city.replaceAll(" ", ""),
//     },
//     query: {
//       lat: searchResult.geometry.coordinates[1],
//       long: searchResult.geometry.coordinates[0],
//       preview: true,
//     },
//   });
// };

// V6
const previewCity = (searchResult) => {
  console.log(searchResult);
  const [city, state] = searchResult.properties.full_address.split(",");
  router.push({
    name: "cityView",
    params: {
      state: state.replaceAll(" ", ""),
      city: city.replaceAll(" ", ""),
    },
    query: {
      lat: searchResult.properties.coordinates.latitude,
      long: searchResult.properties.coordinates.longitude,
      preview: true,
    },
  });
};

const mapboxAPIKey =
  "pk.eyJ1IjoiaWJudS1zb2RpayIsImEiOiJjbHcweDdwb2cwNW1rMmpudmtlODZhcTRtIn0.u-iJiu2sUaqQzd49nXw6BQ";
const searchQuery = ref("");
const queryTimout = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimout.value);
  queryTimout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        // V5
        // const resultV5 = await axios.get(
        //   `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        // );
        // mapboxSearchResults.value = resultV5.data.features;

        // V6
        const resultV6 = await axios.get(
          `https://api.mapbox.com/search/geocode/v6/forward?place=${searchQuery.value}&access_token=${mapboxAPIKey}`
        );
        mapboxSearchResults.value = resultV6.data.features;
      } catch {
        searchError.value = true;
      }
      return;
    }
    mapboxSearchResults.value = null;
  }, 3000);
};
</script>

<style lang="scss" scoped></style>
