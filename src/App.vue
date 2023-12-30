<script setup lang="ts">
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import { ref, onMounted } from 'vue';
import { db, GuitarProps } from './data/guitars';
import Guitar from "./components/Guitar.vue";

const guitars = ref<GuitarProps[]>([]);

onMounted(() => {
  guitars.value = db;
});
</script>

<template>
  <Header />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar 
        v-for="guitar in guitars" :guitar="guitar"/>
    </div>
  </main>

  <Footer />
</template>

<style scoped></style>
