<script setup>
import { ref } from "vue";
import ProductDisplay from '@/components/ProductDisplay.vue'
const cart = ref(0);
</script>

<template>
  <div class="nav-bar">
    <div class="cart">cart({{ cart }})</div>
    <ProductDisplay />
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
