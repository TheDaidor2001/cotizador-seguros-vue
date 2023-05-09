<script setup>
    const props = defineProps({
        type: String
    })
</script>



<template>
    <button type="button"
        class="w-10 h-10 bg-lime-500 rounded-full text-white text-2xl font-bold flex items-center justify-center"
        @click="$emit('fn')">
        {{ type }}
    </button>
</template>