<script setup>
    import { ref } from 'vue';
    import greenSocksImage from './assets/images/socks_green.jpeg';
    import blueSocksImage from './assets/images/socks_blue.jpeg';

    const product = ref("Socks")
    const image = ref(greenSocksImage)
    const inStock = ref(false)
    const details = ref([ "50% cotton", "30% wool", "20% polyster" ])
    const variants = ref([
        { id: 2234, color: "green", image: greenSocksImage, quantity: 50 },
        { id: 2235, color: "blue", image: blueSocksImage, quantity: 0 }
    ])
    const cart = ref(0)
    
    const addToCart = () => cart.value += 1;
    const updateImage = (selectedImage) => image.value = selectedImage;

</script>

<template>
    <div class="nav-bar">
        <div class="cart">cart({{ cart }})</div>
        <div class="product-display">
            <div class="product-container">
                <div class="product-image">
                    <img v-bind:src="image" alt="">
                </div>                
                <div class="product-info">
                    <h1>{{ product }}</h1>
                    <p v-if="inStock">In Stock</p>
                    <p v-else>Out of Stock</p>
                    <ul>
                        <li v-for="(detail, index) in details" :key="index">{{ detail }}</li>
                    </ul>
                    <div 
                        v-on:mouseover="updateImage(variant.image)" 
                        v-for="variant in variants" :key="variant.id" 
                        class="color-circle"
                        :style="{backgroundColor: variant.color}"
                    ></div>
                    <!-- optional function wrapper
                    <button class="button" v-on:click="(event) =>{ cart += 1 }">Add to cart</button> -->
                    <!-- <button v-on:click="cart += 1" class="button">Add to cart</button> -->
                    <button 
                        class="button" 
                        :class="{ disabledButton : !inStock }"
                        :disabled="!inStock"
                        @click="addToCart"
                    >Add to cart</button>
                </div>
            </div>
        </div>
    </div>
</template>

<!-- ============== Reactive ================ -->
<!-- another way to create state in vue: reactive() -->
<!-- If you have a bunch of related data they usually go together, you can you reactive -->
<!-- <script setup>
    import { reactive } from 'vue';

    const product = reactive({
        name: "Socks"
    })

    setTimeout(() =>{
        product.name = "New Socks"
    }, 1000)
</script>

<template>
    <h1>{{ product.name }}</h1>
</template> -->

<!-- ================  Composition API(script setup) ================ -->
<!-- Composition API with the script setup(latest syntax) -->
<!-- Composition API (script setup) is vue.js equivalent of React Hooks -->
<!-- Using composition api, we can create custom hooks. But in Vue.js worlds, we call them Composables -->
<!-- <script setup>
import { ref } from 'vue'
const product = ref("Socks")

setTimeout(() => {
    product.value = "New Socks"
}, 1000)

</script>

<template>
    <h1> {{ product }} </h1>
</template> -->


<!-- =============== Composition API (classic) =============== -->
<!-- classic composition api syntax -->
<!-- <script>
import { ref } from 'vue';
export default {
    setup() {
        const product = ref("Socks")

        setTimeout(() => {
            product.value = "New Socks"
        }, 1000)

        return { product }
    }
}
</script>

<template>
    <h1>{{ product }}</h1>
</template> -->


<!-- =================== Options API ============== -->
<!-- Options API is the vue.js equivalent of React's class-based API -->
<!-- <script>
    export default{
        data() {
            return {
                product: "Socks"
            }
        },
        created() {
            setTimeout(() => {
                this.product = "New Socks"
            }, 1000)
        }
    }
</script>

<template>
    <h1>{{ product }}</h1>
</template> -->