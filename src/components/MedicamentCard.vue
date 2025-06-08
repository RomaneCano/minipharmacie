<script setup>
const props = defineProps({
  medicament: Object,
  onSupprimer: Function,
  onModifierQuantite: Function,
  onDemanderModification: Function
});

const emojiQuantite = (quantite) => {
  if (quantite >= 10) return "😃";
  if (quantite >= 5) return "😐";
  return "😟";
};
</script>

<template>
  <li class="carte-medicament">
    <h3>{{ medicament.denomination }}</h3>
    <p>Forme : {{ medicament.formepharmaceutique }}</p>
    <p>Quantité : {{ medicament.qte }} {{ emojiQuantite(medicament.qte) }}</p>
    <p v-if="medicament.qte === 0" style="color: red; font-weight: bold;">
      ⚠️ Ce médicament est en rupture de stock
    </p>
    <img
      :src="'https://apipharmacie.pecatte.fr/images/' + medicament.photo"
      alt="image"
      width="100"
    />
    <button @click="onModifierQuantite(medicament, 1)">+1</button>
    <button @click="onModifierQuantite(medicament, -1)">-1</button>
    <button @click="onSupprimer(medicament.id)">Supprimer</button>
    <button @click="onDemanderModification(medicament)">✏ Modifier</button>
  </li>
</template>

<style scoped>
.carte-medicament {
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 15px;
  margin-bottom: 10px;
}
</style>
