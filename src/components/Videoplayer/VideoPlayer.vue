<script setup>
    //https://vue-3-tiktok.vercel.app/video1.mp4
    import { ref,defineProps, reactive, onMounted } from "vue";
    import 'animate.css'; 

    const props = defineProps({
        src: {
            type: String,
            required: true
        }
    });

    const emit = defineEmits(['playPrev', 'playNext']);

</script>

<template>
    <div class="player">
        <Transition
            mode="out-in"
            enter-active-class="animate__animated animate__fadeIn"
            leave-active-class="animate__animated animate__fadeOut">
        <video :key="props.src" :src="props.src" autoplay muted loop></video>
        </Transition>

        <div class="controls">
            <a @click.prevent="$emit('playPrev')" href="#">&#x2191;</a>
            <a @click.prevent="$emit('playNext')" href="#">&#x2193;</a>
        </div>
    </div>
</template>

<style scoped>
    video {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .controls {
        position: absolute;
        bottom: 10px;
        right: 10px;
        display: flex;
        flex-direction: column;
    }

    .player {
        position: relative;
        width: 100%;
        height: 100%;
    }
 
    .controls {
        position: absolute;
        height: 100%;
        top: 0;
        right: 2px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        font-size: 1.5rem;
    }
 
    .controls a {
        text-decoration: none;
        color: white;
        margin: 0.5rem 0;
        font-weight: bold;
    }

</style>