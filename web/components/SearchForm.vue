<template>
  <form @submit.prevent="handleSearch" class="flex justify-center mt-4">
    <div class="relative w-80">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Enter city name"
        class="w-full p-2 pr-12 border border-white text-white rounded bg-transparent focus:outline-none focus:ring-2 focus:ring-blue-500"
      />
      <button 
        type="submit"
        class="absolute right-2 top-1/2 transform -translate-y-1/2 bg-blue-500 text-white px-3 py-1 rounded hover:bg-blue-600 transition cursor-pointer"
        :class="searchQuery ? 'bg-blue-500 hover:bg-blue-600' : 'bg-gray-400 cursor-not-allowed'"
        :disabled="!searchQuery"
      >
        🔍
      </button>
    </div>
  </form>
  <p v-if="error" class="text-red-500 mt-2">{{ error }}</p>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useWeatherStore } from '@/stores/weatherStore';

const router = useRouter();
const searchQuery = ref('');
const error = ref('');
const weatherStore = useWeatherStore();

const handleSearch = async () => {
  error.value = '';
  try {
    await weatherStore.fetchWeather(searchQuery.value);
    if (weatherStore.weather) {
      router.push(`/weather/${searchQuery.value}`);
    }
  } catch (err) {
      error.value = err.response?._data?.message;
    }
};

</script>
