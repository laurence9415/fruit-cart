<script setup>
import { computed, onMounted, ref } from 'vue'
import { debounce } from 'lodash'
import InputSearch from '../Components/InputSearch.vue'
import Product from '../Components/Product.vue'

const fruits = ref([])
const search = ref('')
const products = ref([])


const clickFruit = (payload) => {
    products.value.push({
        ...payload,
        count: 1
    })
    search.value = ''
}

const decrementProduct = (productId) => {
    if (products.value.some(product => product.id === productId && product.count === 1)) {
        products.value = products.value.filter(product => product.id !== productId)

        return
    }

    products.value = products.value.map(product => {
        if (product.id === productId) {
            product.count--
        }

        return product
    })
}

const incrementProduct = (productId) => {
    products.value = products.value.map(product => {
        if (product.id === productId) {
            product.count++
        }

        return product
    })
}

const filteredFruits = computed(() => {
    if (!search.value) return []

    return fruits.value.filter(fruit => fruit.name.toLowerCase().indexOf(search.value.toLowerCase()) >= 0 && !products.value.some(product => product.id === fruit.id))
})


onMounted(() => {
    axios.get(`/api/fruits`)
        .then(response => {
            fruits.value = response.data
        })
})
</script>

<template>
    <div class="py-6 px-8">
        <h1 class="text-2xl mb-8">Add Products</h1>
        <div>
            <label for="product">Product</label>
            <div class="flex gap-2">
                <InputSearch v-model:modelValue="search" :options="filteredFruits" @selected="clickFruit" />
                <button class="bg-sky-600 text-white py-2 px-4 rounded">Add</button>
            </div>

            <div class="mt-8">
                <template v-if="products.length > 0">
                    <template v-for="product in products">
                        <Product :product="product" @decrement="decrementProduct(product.id)"
                            @increment="incrementProduct(product.id)" />
                    </template>
                </template>
                <div class="text-center w-full" v-else>
                    <label class="text-center">No products have been added</label>
                </div>
            </div>

        </div>
    </div>
</template>

<style></style>