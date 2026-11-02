<script setup>
    defineProps({
        messages: {
            type: Array,
            required: true
        },
        loading: {
            type: Boolean,
            default: false
        }
    })
</script>

<template>
    <div class="chat-messages">
        <p v-if="loading">Commentaren laden...</p>
        <p v-else-if="messages.length === 0">Nog geen commentaren.</p>
        <ul v-else>
            <li v-for="message in messages" :key="message._id">
                <strong>{{ message.user }}</strong>
                <p>{{ message.text }}</p>
            </li>
        </ul>
    </div>
</template>

<style scoped>
    .chat-messages {
        margin-top: 1rem;
        max-height: 50vh;
        overflow-y: auto;
    }

    ul {
        margin: 0;
        padding: 0;
        list-style: none;
    }

    li {
        padding: 0.6rem 0;
        border-bottom: 1px solid #ddd;
    }

    li p {
        margin: 0.2rem 0 0;
        overflow-wrap: anywhere;
    }
</style>
