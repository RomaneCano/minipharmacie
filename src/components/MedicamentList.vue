<script setup>
import { ref, onMounted } from 'vue';
import CarteMedicament from './MedicamentCard.vue';
import FormulaireMedicament from './MedicamentForm.vue';
import RechercheMedicament from './MedicamentSearch.vue';

const ID_PHARMACIE = "27";
const URL_API = `https://apipharmacie.pecatte.fr/api/${ID_PHARMACIE}/medicaments`;

const recherche = ref('');
const listeMedicaments = ref([]);
const messageAccueil = ref("Bienvenue dans votre pharmacie !");
const medicamentAModifier = ref(null);
const nomModifie = ref("");
const formeModifiee = ref("");
const qteModifiee = ref(1);
const nouvelleImageBase64 = ref("");

const chargerMedicaments = () => {
  const url = recherche.value ? `${URL_API}?search=${recherche.value}` : URL_API;
  fetch(url)
    .then(res => res.json())
    .then(donnees => listeMedicaments.value = donnees);
};

const ajouterMedicament = (medicament) => {
  fetch(URL_API, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(medicament)
  }).then(chargerMedicaments);
};

const supprimerMedicament = (id) => {
  fetch(`${URL_API}/${id}`, { method: "DELETE" })
    .then(chargerMedicaments);
};

const modifierQuantite = (medicament, delta) => {
  if (medicament.qte + delta < 0) return;

  const miseAJour = {
    ...medicament,
    qte: medicament.qte + delta,
    id: medicament.id
  };

  fetch(URL_API, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(miseAJour)
  }).then(chargerMedicaments);
};

const demanderModification = (med) => {
  medicamentAModifier.value = med;
  nomModifie.value = med.denomination;
  formeModifiee.value = med.formepharmaceutique;
  qteModifiee.value = med.qte;
  nouvelleImageBase64.value = '';
};

const importerNouvelleImage = (event) => {
  const fichier = event.target.files[0];
  if (!fichier) return;
  const lecteur = new FileReader();
  lecteur.onload = () => {
    nouvelleImageBase64.value = lecteur.result.split(",")[1];
  };
  lecteur.readAsDataURL(fichier);
};

const sauvegarderModification = () => {
  if (!medicamentAModifier.value) return;

  const donnees = {
    id: medicamentAModifier.value.id,
    denomination: nomModifie.value,
    formepharmaceutique: formeModifiee.value,
    qte: qteModifiee.value,
    photo: nouvelleImageBase64.value || medicamentAModifier.value.photo?.split("/").pop()
  };

  fetch(URL_API, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(donnees)
  }).then(() => {
    chargerMedicaments();
    medicamentAModifier.value = null;
  });
};

onMounted(() => {
  chargerMedicaments();
  setTimeout(() => {
    messageAccueil.value = "";
  }, 3000);
});
</script>

<template>
  <div>
    <p v-if="messageAccueil" style="text-align:center; font-style:italic; color:green;">
      {{ messageAccueil }}
    </p>

    <RechercheMedicament v-model="recherche" @rechercher="chargerMedicaments" />
    <FormulaireMedicament @ajouter="ajouterMedicament" />

    <ul>
      <li v-for="m in listeMedicaments" :key="m.id" style="display: flex; align-items: start; gap: 20px; margin-bottom: 20px;">
        <CarteMedicament
          :medicament="m"
          @supprimer="supprimerMedicament"
          @modifierQuantite="modifierQuantite"
          @demanderModification="demanderModification"
        />

        <div v-if="medicamentAModifier && medicamentAModifier.id === m.id" class="modif-container" style="border-left: 1px solid #ccc; padding-left: 10px;">
          <h4>Modifier</h4>
          <input type="text" v-model="nomModifie" placeholder="Nom" />
          <input type="text" v-model="formeModifiee" placeholder="Forme" />
          <input type="number" v-model="qteModifiee" min="0" placeholder="Quantité" />
          <input type="file" @change="importerNouvelleImage" />
          <button @click="sauvegarderModification">💾</button>
          <button @click="medicamentAModifier = null">❌</button>
        </div>
      </li>
    </ul>

    <p v-if="listeMedicaments.length === 0" style="text-align:center; color:gray;">
      Aucun médicament trouvé.
    </p>
  </div>
</template>

