<template>
  <div class="p-4 min-h-screen bg-gray-50">
    <h1 class="text-lg font-bold mb-4">
      Task 5: Data Caching in an API Call
    </h1>

    <div class="mb-4">
      <input
        v-model="userId"
        type="number"
        max="5"
        @input="fetchUser"
        placeholder="Enter user ID"
        class="border rounded px-2 py-1"
      />
    </div>

    <div v-if="loading" class="text-blue-500">Loading...</div>
    <div v-if="error" class="text-red-500">{{ error }}</div>

    <div v-if="userData" class="mt-4 p-4 bg-white rounded shadow">
      <pre>{{ userData }}</pre>
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

<style scoped></style>
