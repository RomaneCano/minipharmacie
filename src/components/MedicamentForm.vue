<script setup>
import { ref } from 'vue';

const nom = ref('');
const forme = ref('');
const quantite = ref(1);
const photoBase64 = ref('');

const emettre = defineEmits(['ajouter']);

const televerserImage = (e) => {
  const fichier = e.target.files[0];
  if (!fichier) return;
  const lecteur = new FileReader();
  lecteur.onload = () => {
    photoBase64.value = lecteur.result.split(',')[1];
  };
  lecteur.readAsDataURL(fichier);
};

const ajouter = () => {
  emettre('ajouter', {
    denomination: nom.value,
    formepharmaceutique: forme.value,
    qte: quantite.value,
    photo: photoBase64.value
  });

  nom.value = '';
  forme.value = '';
  quantite.value = 1;
  photoBase64.value = '';
};

const reinitialiserChamps = () => {
  nom.value = '';
  forme.value = '';
  quantite.value = 1;
  photoBase64.value = '';
};

</script>

<template>
  <div>
    <h2>Ajouter un médicament</h2>
    <input v-model="nom" placeholder="Nom" />
    <input v-model="forme" placeholder="Forme" />
    <input type="file" @change="televerserImage" />
    <input type="number" v-model="quantite" min="0" />
    <button @click="ajouter">Ajouter</button>
    <button @click="reinitialiserChamps" type="button">Réinitialiser</button>
    <button @click="chargerMedicaments" style="margin: 10px;">Recharger la liste</button>


  </div>
</template>
