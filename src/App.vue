<script setup>
import ListeMedicaments from './components/MedicamentList.vue';
</script>

<template>
  <div class="app">
    <h1>Gestion de la Pharmacie</h1>
    <ListeMedicaments />
  </div>
</template>

<style scoped>
.app {
  padding: 20px;
  max-width: 800px;
  margin: auto;
  font-family: Arial, sans-serif;
}
</style>
