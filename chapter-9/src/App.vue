<template>
  <h1>{{ prenom }} {{ nom }}</h1>
  <Modal v-model:prenom="prenom" v-model:nom="nom" />

  <Enfant>
    <template #header>
      <h1>Titre de l'article</h1>
    </template>
    <p>
      Le Lorem Ipsum est simplement du faux texte employé dans la composition et
      la mise en page avant impression. Le Lorem Ipsum est le faux texte
      standard de l'imprimerie depuis les années 1500, quand un imprimeur
      anonyme assembla ensemble des morceaux de texte pour réaliser un livre
      spécimen de polices de texte. Il n'a pas fait que survivre cinq siècles,
      mais s'est aussi adapté à la bureautique informatique, sans que son
      contenu n'en soit modifié. Il a été popularisé dans les années 1960 grâce
      à la vente de feuilles Letraset contenant des passages du Lorem Ipsum, et,
      plus récemment, par son inclusion dans des applications de mise en page de
      texte, comme Aldus PageMaker.
    </p>
    <template #[bottom]>
      <AppButton></AppButton>
    </template>
  </Enfant>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Modal from './components/Modal.vue';
import Enfant from './components/Enfant.vue';
import AppButton from './components/AppButton.vue';

const bottom = 'cta';

const prenom = ref('');
const nom = ref('');
</script>

<style scoped lang="scss"></style>
