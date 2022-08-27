<script setup>

defineProps(['inicio', 'fin', 'maxLength'])

</script>

<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button 
            @click="$emit('previous')" 
            type="button" 
            class="btn btn-outline-primary"
            :disabled="inicio <= 0"
        >
            Previous {{ inicio }}
        </button>
        <button
            @click="$emit('next')"
            type="button"
            class="btn btn-outline-primary"
            :disabled="fin >= maxLength"
        >
            Next {{ fin }}
        </button>
    </div>
</template>