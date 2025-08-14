<template>
  <div class="p-6 min-h-screen bg-gradient-to-b from-gray-100 to-gray-200 flex flex-col items-center">
    <h1 class="text-3xl font-extrabold mb-8 text-gray-800">Task 5: Data Caching API</h1>

    <!-- Input -->
   <div class="mb-8 w-full max-w-sm">
  <label for="userId" class="block text-gray-700 font-medium mb-2">
    Enter User ID (1-10)
  </label>
  <input
    id="userId"
    v-model="userId"
    type="number"
    max="10"
    @input="fetchUser"
    placeholder="Type a number between 1 and 10"
    class="w-full px-4 py-3 border border-gray-300 rounded-xl shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400 focus:border-transparent transition"
  />
</div>


    <!-- Loading / Error -->
    <div v-if="loading" class="text-blue-500 font-medium mb-4 animate-pulse">Loading user data...</div>
    <div v-if="error" class="text-red-500 font-medium mb-4">{{ error }}</div>

    <!-- User Card -->
    <div
      v-if="userData"
      class="w-full max-w-lg bg-white rounded-2xl shadow-xl overflow-hidden transform hover:scale-105 transition-transform duration-300"
    >
      <!-- Header -->
      <div class="bg-gradient-to-r from-blue-400 to-indigo-500 text-white px-6 py-5 flex items-center justify-between">
        <div>
          <h2 class="text-2xl font-bold">{{ userData.name }}</h2>
          <p class="text-sm opacity-90">@{{ userData.username }}</p>
        </div>
        <div class="flex space-x-3">
          <a :href="`mailto:${userData.email}`" class="hover:text-yellow-200 transition">
            <i class="fas fa-envelope"></i>
          </a>
          <a :href="`tel:${userData.phone}`" class="hover:text-yellow-200 transition">
            <i class="fas fa-phone"></i>
          </a>
          <a :href="`http://${userData.website}`" target="_blank" class="hover:text-yellow-200 transition">
            <i class="fas fa-globe"></i>
          </a>
        </div>
      </div>

      <!-- Body -->
      <div class="px-6 py-5 space-y-4">
        <!-- Email / Phone / Website -->
        <div class="flex flex-col space-y-1 text-gray-700">
          <div><span class="font-semibold">Email:</span> {{ userData.email }}</div>
          <div><span class="font-semibold">Phone:</span> {{ userData.phone }}</div>
          <div><span class="font-semibold">Website:</span> {{ userData.website }}</div>
        </div>

        <!-- Address -->
        <div class="bg-gray-50 p-4 rounded-xl shadow-inner">
          <h3 class="font-semibold text-gray-800 mb-1">Address</h3>
          <p>{{ userData.address.street }}, {{ userData.address.suite }}</p>
          <p>{{ userData.address.city }}, {{ userData.address.zipcode }}</p>
        </div>

        <!-- Company -->
        <div class="bg-gray-50 p-4 rounded-xl shadow-inner">
          <h3 class="font-semibold text-gray-800 mb-1">Company</h3>
          <p class="font-medium">{{ userData.company.name }}</p>
          <p class="text-sm italic text-gray-600">{{ userData.company.catchPhrase }}</p>
          <p class="text-sm text-gray-500">{{ userData.company.bs }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const userData = ref(null);
const userId = ref("");
const loading = ref(false);
const error = ref(null);

const cache = new Map();
let currentController = null;

const fetchUser = async () => {
  const id = userId.value;
  if (!id) return;

  if (cache.has(id)) {
    userData.value = cache.get(id);
    error.value = null;
    loading.value = false;
    return;
  }

  if (currentController) currentController.abort();

  currentController = new AbortController();
  loading.value = true;
  error.value = null;
  userData.value = null;

  try {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/users/${id}`,
      { signal: currentController.signal }
    );
    userData.value = response.data;
    cache.set(id, response.data);
  } catch (err) {
    if (err.name !== "CanceledError") {
      error.value = "Failed to fetch user";
      console.log(err);
    }
  } finally {
    loading.value = false;
  }
};
</script>

<style scoped>
/* Include FontAwesome for icons */
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css');
</style>
