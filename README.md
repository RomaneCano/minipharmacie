# README – Application de gestion de pharmacie

Ce projet a été réalisé dans le cadre du mini-projet de technologies du web. Il s'agit d'une application web développée avec le framework Vue.js, qui permet de gérer les stocks de médicaments d'une pharmacie à l'aide d'une API REST fournie.

## Fonctionnalités de base

L’application implémente l’ensemble des fonctionnalités attendues :

* Affichage de la liste des médicaments de la pharmacie.
* Recherche dynamique de médicaments par leur dénomination.
* Ajout d’un médicament avec les champs suivants : dénomination, forme pharmaceutique, quantité initiale, et image.
* Suppression complète d’un médicament, quelle que soit sa quantité.
* Incrémentation (+1) ou décrémentation (−1) de la quantité d’un médicament.
* Modification des informations d’un médicament existant (nom, forme, quantité, photo).

Toutes ces fonctionnalités interagissent avec l’API distante accessible à l’adresse suivante :
`https://apipharmacie.pecatte.fr/api/1/medicaments` (ID pharmacie : 1, correspondant à mon rang dans la liste d'appel)

## Fonctionnalités supplémentaires

Des fonctionnalités additionnelles ont été développées pour améliorer l’expérience utilisateur :

* **Message de bienvenue** : affiché au chargement de l'application, il disparaît automatiquement après quelques secondes.
* **Smiley en fonction de la quantité** : un emoji est affiché pour indiquer l’état du stock :

  * 😃 si la quantité est supérieure ou égale à 10
  * 😐 entre 5 et 9
  * 😟 en dessous de 5
* **Alerte de rupture de stock** : lorsqu’un médicament a une quantité nulle, un message d’avertissement s’affiche sous sa fiche.
* **Bouton d’enregistrement des modifications** : lors de la modification d’un médicament, un bouton permet d’enregistrer les changements effectués.
* **Réinitialisation du formulaire** : un bouton dans le formulaire d’ajout permet de réinitialiser tous les champs saisis.
* **Message “Aucun médicament trouvé”** : si la recherche ne renvoie aucun résultat, ce message est affiché à l’utilisateur.

## Technologies utilisées

* Vue.js 3 
* HTML / CSS
* API REST externe

## Utilisation

Pour exécuter le projet en local :

```bash
npm install
npm run dev
```

## Structure de l’application

L'application est composée des fichiers et composants suivants :

* `App.vue` : composant racine
* `MedicamentList.vue` : composant principal, contenant toute la logique métier et les appels à l’API
* `MedicamentCard.vue` : carte affichant un médicament avec ses actions (modifier, supprimer, changer quantité)
* `MedicamentForm.vue` : formulaire pour ajouter un médicament
* `MedicamentSearch.vue` : champ de recherche connecté avec `v-model`
* `Medicament.js` : classe représentant un médicament

## Déploiement

Le projet a été déployé avec AlwaysData.

