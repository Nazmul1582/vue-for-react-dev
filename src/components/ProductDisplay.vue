<script setup>
import { ref, computed } from "vue";
import greenSocksImage from "../assets/images/socks_green.jpeg";
import blueSocksImage from "../assets/images/socks_blue.jpeg";

const brand = ref("Premium");
const product = ref("Socks");
const details = ref(["50% cotton", "30% wool", "20% polyster"]);
const variants = ref([
  { id: 2234, color: "green", image: greenSocksImage, quantity: 50 },
  { id: 2235, color: "blue", image: blueSocksImage, quantity: 0 },
]);
const selectedVariant = ref(0);

const addToCart = () => (cart.value += 1);

const title = computed(() => {
  return brand.value + " " + product.value;
});
const image = computed(() => {
  return variants.value[selectedVariant.value].image;
});
const inStock = computed(() => {
  return variants.value[selectedVariant.value].quantity > 0;
});
const updateVariant = (index) => {
  selectedVariant.value = index;
};
</script>

<template>
  <div class="product-display">
    <div class="product-container">
      <div class="product-image">
        <img v-bind:src="image" alt="" />
      </div>
      <div class="product-info">
        <h1>{{ title }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <ul>
          <li v-for="(detail, index) in details" :key="index">{{ detail }}</li>
        </ul>
        <div
          v-for="(variant, index) in variants"
          :key="variant.id"
          v-on:mouseover="updateVariant(index)"
          class="color-circle"
          :style="{ backgroundColor: variant.color }"
        ></div>
        <!-- optional function wrapper
                <button class="button" v-on:click="(event) =>{ cart += 1 }">Add to cart</button> -->
        <!-- <button v-on:click="cart += 1" class="button">Add to cart</button> -->
        <button
          class="button"
          :class="{ disabledButton: !inStock }"
          :disabled="!inStock"
          @click="addToCart"
        >
          Add to cart
        </button>
      </div>
    </div>
  </div>
</template>
