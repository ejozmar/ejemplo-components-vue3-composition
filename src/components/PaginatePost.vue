<script setup>
defineProps(['inicio', 'fin', 'totalPosts']);

const emit = defineEmits([
    'next',
    'prev'
]);
</script>

<template>
    <div class="btn-group btn-group-lg" role="group" aria-label="Large button group">
        <button 
            @click="emit('prev')" 
            type="button" 
            class="btn btn-outline-primary"
            :disabled="inicio <= 0"
        >
            Previous
        </button>
        <button 
            @click="emit('next')" 
            type="button" 
            class="btn btn-outline-primary"
            :disabled="fin >= totalPosts"
        >
            Next {{ fin }}
        </button>
    </div>
</template>