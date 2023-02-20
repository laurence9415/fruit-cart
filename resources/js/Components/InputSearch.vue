<script setup>
import { onMounted, ref } from 'vue'

const emit = defineEmits(['selected'])

const props = defineProps({
    modelValue: {
        type: String,
        default: '',
    },
    options: {
        type: Array,
        default: () => []
    }
})

const fruitsSelection = ref()

const setSearchFruits = () => {
    const typeahead = document.getElementById('typeahead')
    fruitsSelection.value.style.width = `${typeahead.clientWidth}px`
}

const handleClick = (option) => {
    emit('selected', option)
}

onMounted(() => {
    setSearchFruits()
})
</script>

<template>
    <div class="flex-1">
        <input id="typeahead" type="text" class="w-full rounded" placeholder="e.g Product ABC"
            @input="$emit('update:modelValue', $event.target.value)" :value="modelValue">
        <div ref="fruitsSelection" class="fruits border p-2" :class="{ 'hidden': !modelValue }">
            <ul>
                <li class="fruit" v-for="option in options" @click="handleClick(option)">{{ option.name }}</li>
            </ul>
        </div>
    </div>
</template>

<style>
.fruits {
    position: absolute;
    z-index: 5;
    background: white;
}

.fruit {
    cursor: pointer;
}

.fruit:hover {
    background-color: #ccc;
}
</style>