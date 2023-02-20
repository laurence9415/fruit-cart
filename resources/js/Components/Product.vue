<script setup>
const emit = defineEmits(['increment', 'decrement'])

defineProps({
    product: {
        type: Object,
        required: true,
    }
})

const handleClick = (action) => {
    emit(action)
}

</script>

<template>
    <div class="flex border p-4 my-2">
        <label class="flex-1 m-0">{{ product.name }}</label>
        <div class="flex gap-10">
            <button class="text-2xl" @click="handleClick('decrement')">-</button>
            <span class="bg-gray-300 p-2 m-0">{{ product.count }}</span>
            <button class="text-2xl" @click="handleClick('increment')">+</button>
        </div>
    </div>
</template>

<style></style>