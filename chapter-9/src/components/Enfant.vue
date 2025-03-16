<template>
    <div class="container">
      <div class="title">
        <slot name="header"></slot>
      </div>
      <section>
        <slot name="default"></slot>
      </section>
      <div class="cta">
        <slot name="cta"></slot>
      </div>
    </div>
  </template>
  
  <script setup lang="ts"></script>
  
  <style scoped lang="scss">
  .container {
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 20px;
    width: 500px;
    :slotted(h1) {
      color: gray;
    }
  }
  </style>
  