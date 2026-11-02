<script setup>
    import {defineProps} from "vue";

    const props = defineProps({
        description: {
            type: String,
            required: true
        },
        username: {
            type: String,
            required: true
        }
    });

    
</script>

<template>
    <div>
        <h3>{{ props.username }}</h3>
        <p>{{ props.description }}</p></div>
</template>

<style scoped>

</style>