<script setup>
defineProps(['medicament']);
const emettre = defineEmits(['supprimer', 'modifierQuantite', 'demanderModification']);

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
    <img v-bind:src="'https://apipharmacie.pecatte.fr/images/' + medicament.photo"
      alt="image" width="100" />
    <button @click="emettre('modifierQuantite', medicament, 1)">+1</button>
    <button @click="emettre('modifierQuantite', medicament, -1)">-1</button>
    <button @click="emettre('supprimer', medicament.id)">Supprimer</button>
    <button @click="emettre('demanderModification', medicament)">✏ Modifier</button>

  </li>
</template>
