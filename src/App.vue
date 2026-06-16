<script setup>
import FoodList from './components/FoodList.vue'
</script>

<template>
  <main class="app">
    <h1>FoodTracker</h1>
    <FoodList />
  </main>
</template>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f5f7f8;
  color: #1f2933;
}

.app {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px 24px;
}

h1 {
  margin-bottom: 32px;
}
</style>
