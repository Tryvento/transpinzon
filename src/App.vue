<script setup>
import MainLayout from "./layouts/MainLayout.vue";

</script>

<template>
  <MainLayout class="layout">
    <router-view />
  </MainLayout>
</template>

<style scoped>
.layout {
  scroll-behavior: smooth;
}
</style>
