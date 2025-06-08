<script setup>
const recherche = defineModel(); 
</script>

<template>
  <div>
    <input
      v-model="recherche"
      placeholder="Rechercher un médicament"
    />
  </div>
</template>

<style scoped>
input {
  width: 100%;
  margin-bottom: 20px;
}
</style>
