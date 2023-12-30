<script setup lang="ts">
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import { ref, onMounted } from 'vue';
import { db, GuitarProps } from './data/guitars';
import Guitar from './components/Guitar.vue';

export type GuitarWithQuantity = GuitarProps & {
  quantity: number;
};

const guitars = ref<GuitarProps[]>([]);
const cart = ref<GuitarWithQuantity[]>([]);

onMounted(() => {
  guitars.value = db;
});

const addToCart = (guitar: GuitarProps) => {
  const existInCart = cart.value.findIndex(
    (product) => product.id === guitar.id
  );
  if (existInCart >= 0) {
    cart.value[existInCart].quantity++;
  } else {
    const guitarWithQuantity: GuitarWithQuantity = { ...guitar, quantity: 1 };
    cart.value.push(guitarWithQuantity);
  }
};
</script>

<template>
  <Header :cart="cart" />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        :guitar="guitar"
        @add-to-cart="addToCart"
      />
    </div>
  </main>

  <Footer />
</template>

<style scoped></style>
