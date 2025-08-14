<template>
  <div class="flex flex-col min-h-screen font-sans bg-gray-50">
    <!-- Header / Navbar -->
    <header class="bg-white shadow-md">
      <div class="container mx-auto flex items-center justify-between py-4 px-6">
        <h1 class="text-lg font-bold text-blue-600">🚀 My Challenge</h1>

        <!-- Navigation Links -->
        <nav class="space-x-6">
          <RouterLink to="/" class="text-gray-700 hover:text-blue-600 transition">
            Home
          </RouterLink>
          <RouterLink to="/about" class="text-gray-700 hover:text-blue-600 transition">
            About
          </RouterLink>
          <RouterLink to="/contact" class="text-gray-700 hover:text-blue-600 transition">
            Contact
          </RouterLink>
        </nav>
      </div>
    </header>

    <!-- Main Content -->
    <main class="flex-grow container mx-auto px-6 py-8">
      <RouterView />
    </main>

    <!-- Footer -->
    <footer class="bg-gray-800 text-gray-300">
      <div
        class="container mx-auto px-6 py-6 flex flex-col sm:flex-row justify-between items-center"
      >
        <p class="text-sm">© 2025 Ali Aslani — All rights reserved.</p>
        <div class="space-x-4 mt-4 sm:mt-0">
          <a href="#" class="hover:text-white transition">Privacy Policy</a>
          <a href="#" class="hover:text-white transition">Terms</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup></script>

<style scoped>
/* Optional: Improve link transitions */
a {
  text-decoration: none;
}
</style>
