<script setup>
    const username = defineModel('username', { type: String, required: true })
    const text = defineModel('text', { type: String, required: true })

    defineProps({
        sending: {
            type: Boolean,
            default: false
        }
    })

    const emit = defineEmits(['send'])
</script>

<template>
    <form class="chat-form" @submit.prevent="emit('send')">
        <input
            v-model="username"
            type="text"
            placeholder="Gebruikersnaam"
            aria-label="Gebruikersnaam"
            required
        />
        <input
            v-model="text"
            type="text"
            placeholder="Schrijf een commentaar..."
            aria-label="Commentaar"
            required
        />
        <button type="submit" :disabled="sending">
            {{ sending ? 'Versturen...' : 'Versturen' }}
        </button>
    </form>
</template>

<style scoped>
    .chat-form {
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
    }

    input,
    button {
        padding: 0.6rem;
        border-radius: 6px;
        font: inherit;
    }

    input {
        border: 1px solid #bbb;
    }

    button {
        border: none;
        background: #fe2c55;
        color: white;
        font-weight: bold;
        cursor: pointer;
    }

    button:disabled {
        opacity: 0.6;
        cursor: wait;
    }
</style>
