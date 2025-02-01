<template>
  <div class="mt-8">
    <div class="flex justify-center">
      <h2 class="text-lg font-bold text-white">Saved Cities</h2>
    </div>
    <div v-if="isLoading">
      <Spinner />
    </div>
    <div v-else>
      <p class="text-white text-center" v-if="weatherStore.savedCities.length === 0">
        No cities to display
      </p>
      <ul v-else class="flex justify-center items-start flex-row gap-4 flex-wrap">
        <li
          v-for="(city, index) in weatherStore.savedCities"
          :key="index"
          class="bg-white p-4 rounded shadow-lg w-60 flex flex-col items-center justify-center transition hover:bg-gray-100"
        >
          <div class="text-black font-bold text-center">
            {{ city.name.toUpperCase() }}
          </div>

          <div class="flex flex-col w-full mt-2 gap-2">
            <router-link
              :to="`/weather/${city.name}`"
              class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 transition text-center w-full"
            >
              View Weather
            </router-link>
            <button
              @click="handleDelete(city.name)"
              class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600 transition w-full cursor-pointer"
            >
              Delete
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>


<script setup>
import { useWeatherStore } from '@/stores/weatherStore';
import Spinner from '@/components/Spinner.vue';
import { computed } from 'vue';

const weatherStore = useWeatherStore();
const isLoading = computed(() => weatherStore.isLoading);

const handleDelete = async (cityName) => {
  try {
    await weatherStore.deleteCity(cityName);
  } catch (err) {
    alert(err);
  }
};
</script>
