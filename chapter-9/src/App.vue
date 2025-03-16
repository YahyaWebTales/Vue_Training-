<template>
  <h1>{{ prenom }} {{ nom }}</h1>
  <Modal v-model:prenom="prenom" v-model:nom="nom" />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Modal from './components/Modal.vue';

const prenom = ref('');
const nom = ref('');
</script>

<style scoped lang="scss"></style>
