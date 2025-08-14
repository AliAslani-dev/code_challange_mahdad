<template>
  <div class="user-list">
    <div v-for="(user, index) in users" :key="index" class="user-card">
      <img :src="user.picture.thumbnail" alt="user photo" />
      <p>{{ user?.name?.first }} {{ user?.name?.last }}</p>
    </div>

    <!-- Sentinel element for infinite scroll -->
    <div ref="sential"></div>

    <!-- Loading indicator -->
    <p v-if="loading">Loading...</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const users = ref([]);
const page = ref(1);
const loading = ref(false);
const sential = ref(null);

const fetchUser = async () => {
  if (loading.value) return;

  loading.value = true;
  try {
    const response = await axios.get(`https://randomuser.me/api/?page=${page.value}&results=10`);
    users.value.push(...response?.data?.results);
    page.value++; // increment page for next fetch
  } catch (error) {
    console.log(error);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchUser();

  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          fetchUser();
        }
      });
    },
    {
      root: null, // viewport
      rootMargin: "300px",
      threshold: 1.0,
    }
  );

  if (sential.value) {
    observer.observe(sential.value);
  }
});
</script>

<style scoped>
.user-list {
  max-width: 400px;
  margin: 0 auto;
}

.user-card {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 10px 0;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.user-card img {
  border-radius: 50%;
}
</style>
