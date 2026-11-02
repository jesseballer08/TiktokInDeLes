<script setup>
    import { onMounted, reactive, ref } from 'vue'
    import ChatMessages from './ChatMessages.vue'
    import ChatForm from './ChatForm.vue'

    const API_URL = 'https://lab5-p379.onrender.com/api/v1/messages/'

    // Reactieve array: elke wijziging verschijnt meteen op het scherm
    const messages = reactive([])
    const username = ref('')
    const text = ref('')
    const loading = ref(true)
    const sending = ref(false)
    const error = ref('')

    async function fetchMessages() {
        try {
            const response = await fetch(API_URL)
            if (!response.ok) throw new Error(`Status ${response.status}`)

            // De API geeft de oudste berichten eerst: draai ze om zodat de nieuwste bovenaan staan
            const data = await response.json()
            messages.push(...data.reverse())
        } catch (err) {
            console.error(err)
            error.value = 'De commentaren konden niet geladen worden.'
        } finally {
            loading.value = false
        }
    }

    async function sendMessage() {
        const newMessage = { user: username.value.trim(), text: text.value.trim() }
        if (!newMessage.user || !newMessage.text) return

        sending.value = true
        error.value = ''

        try {
            const response = await fetch(API_URL, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(newMessage)
            })
            const json = await response.json()
            if (!response.ok || json.status !== 'success') throw new Error(json.message)

            // Het opgeslagen bericht bovenaan toevoegen, zonder de pagina te herladen
            messages.unshift(json.data)
            text.value = ''
        } catch (err) {
            console.error(err)
            error.value = 'Je commentaar kon niet verstuurd worden. Probeer het opnieuw.'
        } finally {
            sending.value = false
        }
    }

    onMounted(fetchMessages)
</script>

<template>
    <section class="chat">
        <h2>Commentaren</h2>
        <ChatForm
            v-model:username="username"
            v-model:text="text"
            :sending="sending"
            @send="sendMessage"
        />
        <p v-if="error" class="error" role="alert">{{ error }}</p>
        <ChatMessages :messages="messages" :loading="loading" />
    </section>
</template>

<style scoped>
    .chat {
        padding-top: 1rem;
    }

    h2 {
        margin: 0 0 0.75rem;
        font-size: 1.1rem;
    }

    .error {
        margin: 0.75rem 0 0;
        color: #b00020;
    }
</style>
