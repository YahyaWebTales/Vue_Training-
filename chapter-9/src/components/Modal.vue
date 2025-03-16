<template>
    <input
      type="text"
      :value="prenom"
      @input="$emit('update:prenom', ($event.target as HTMLInputElement).value)"
    />
    <input
      type="text"
      :value="nom"
      @input="$emit('update:nom', ($event.target as HTMLInputElement).value!)"
    />
  </template>
  
  <script setup lang="ts">
  defineProps<{
    prenom: string;
    nom: string;
  }>();
  defineEmits<{
    (e: 'update:prenom', value: string): void;
    (e: 'update:nom', value: string): void;
  }>();
  </script>
  
  <style scoped lang="scss"></style>
  