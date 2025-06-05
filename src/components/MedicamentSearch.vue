<script setup>
const recherche = defineModel();
const emettre = defineEmits(['rechercher']);
</script>

<template>
  <div>
    <input
      v-model="recherche"
      placeholder="Rechercher un médicament"
      @input="emettre('rechercher')"
    />
  </div>
</template>
