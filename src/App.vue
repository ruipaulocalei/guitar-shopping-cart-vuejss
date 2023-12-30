<script setup lang="ts">
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import { ref, onMounted, watch } from 'vue';
import { db, GuitarProps } from './data/guitars';
import Guitar from './components/Guitar.vue';

export type GuitarWithQuantity = GuitarProps & {
  quantity: number;
};

const guitars = ref<GuitarProps[]>([]);
const cart = ref<GuitarWithQuantity[]>([]);

watch(
  cart,
  () => {
    saveInLocalStorage();
  },
  { deep: true }
);

onMounted(() => {
  guitars.value = db;
  const cartInLocalStorage = localStorage.getItem('cart');
  if (cartInLocalStorage) {
    cart.value = JSON.parse(cartInLocalStorage);
  }
});

const saveInLocalStorage = () => {
  localStorage.setItem('cart', JSON.stringify(cart.value));
};

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

const incrementQuantity = (id: number) => {
  const index = cart.value.findIndex((product) => product.id === id);
  cart.value[index].quantity++;
};

const decrementQuantity = (id: number) => {
  const index = cart.value.findIndex((product) => product.id === id);
  if (cart.value[index].quantity <= 1) {
    cart.value = cart.value.filter((product) => product.id !== id);
  } else {
    cart.value[index].quantity--;
  }
};

const clearProduct = (id: number) => {
  cart.value = cart.value.filter((product) => product.id !== id);
};

const clearCart = () => {
  cart.value = [];
};
</script>

<template>
  <Header
    :cart="cart"
    @increment-quantity="incrementQuantity"
    @decrement-quantity="decrementQuantity"
    @clear-product="clearProduct"
    @clear-cart="clearCart"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        :key="guitar.id"
        :guitar="guitar"
        @add-to-cart="addToCart"
      />
    </div>
  </main>

  <Footer />
</template>

<style scoped></style>
