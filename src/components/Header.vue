<script setup lang="ts">
import { GuitarWithQuantity } from '../App.vue';
import { computed } from 'vue';
const props = defineProps<{
  cart: GuitarWithQuantity[];
}>();
defineEmits([
  'decrement-quantity',
  'increment-quantity',
  'clear-product',
  'clear-cart',
]);

const totalAmount = computed(() => {
  return props.cart.reduce(
    (total, product) => total + product.quantity * product.price,
    0
  );
});
</script>
<template>
  <header class="py-5 header" :cart="cart">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav
          class="col-md-6 a mt-5 d-flex align-items-start justify-content-end"
        >
          <div class="carrito">
            <img
              class="img-fluid"
              src="/img/carrito.png"
              alt="imagen carrito"
            />

            <div id="carrito" class="bg-white p-3">
              <p v-if="cart.length == 0" class="text-center">
                The cart is empty
              </p>
              <div v-else>
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Image</th>
                      <th>Name</th>
                      <th>Price</th>
                      <th>Quantity</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="product in cart" :key="product.id">
                      <td>
                        <img
                          class="img-fluid"
                          :src="'/img/' + product.image + '.jpg'"
                          :alt="'imagen guitarra ' + product.name"
                        />
                      </td>
                      <td>{{ product.name }}</td>
                      <td class="fw-bold">${{ product.price }}</td>
                      <td class="flex align-items-start gap-4">
                        <button
                          @click="$emit('decrement-quantity', product.id)"
                          type="button"
                          class="btn btn-dark"
                        >
                          -
                        </button>
                        {{ product.quantity }}
                        <button
                          @click="$emit('increment-quantity', product.id)"
                          type="button"
                          class="btn btn-dark"
                        >
                          +
                        </button>
                      </td>
                      <td>
                        <button
                          @click="$emit('clear-product', product.id)"
                          class="btn btn-danger"
                          type="button"
                        >
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <p class="text-end">
                  Total amount: <span class="fw-bold">${{ totalAmount }}</span>
                </p>
                <button
                  @click="$emit('clear-cart')"
                  class="btn btn-dark w-100 mt-3 p-2"
                >
                  Clear Cart
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">Modelo VAI</h1>
          <p class="mt-5 fs-5 text-white">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Temporibus,
            possimus quibusdam dolor nemo velit quo, fuga omnis, iure molestias
            optio tempore sint at ipsa dolorum odio exercitationem eos inventore
            odit.
          </p>
          <p class="text-primary fs-1 fw-black">$399</p>
          <!-- <button
            type="button"
            class="btn fs-4 bg-primary text-white py-2 px-5"
          >
            Agregar al Carrito
          </button> -->
        </div>
      </div>
    </div>

    <img
      class="header-guitarra"
      src="/img/header_guitarra.png"
      alt="imagen header"
    />
  </header>
</template>
<style></style>
