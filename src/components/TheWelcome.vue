<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    <WelcomeItem
      v-for="(task, i) in tasks"
      :key="i"
      class="overflow-hidden rounded-xl shadow-lg hover:shadow-2xl transition-shadow duration-300"
    >
      <div :class="['h-2 w-full', tasksColors[i % tasksColors.length]]"></div>
      <div class="p-4">
        <h2 class="text-lg font-semibold text-gray-800 mb-2">{{ task.name }}</h2>
        <p class="text-gray-600 text-sm">{{ task.description }}</p>
      </div>
    </WelcomeItem>
  </div>
</template>

<script setup>
import WelcomeItem from "./WelcomeItem.vue";

const tasksColors = [
  "bg-red-400",
  "bg-blue-400",
  "bg-yellow-400",
  "bg-green-400",
  "bg-pink-400",
  "bg-purple-400",
  "bg-gray-400",
];

const tasks = [
  { name: "task1 : something", description: "This is a sample task description." },
  { name: "task2 : something", description: "This is a sample task description." },
  { name: "task3 : something", description: "This is a sample task description." },
  { name: "task4 : something", description: "This is a sample task description." },
  { name: "task5 : something", description: "This is a sample task description." },
  { name: "task6 : something", description: "This is a sample task description." },
  { name: "task7 : something", description: "This is a sample task description." },
];
</script>

<style scoped>
/* Optional: make WelcomeItem display block so the card styles work */
WelcomeItem {
  display: block;
  background-color: white;
}
</style>
