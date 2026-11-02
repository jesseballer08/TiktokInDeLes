<script setup>
import VideoPlayer from './components/Videoplayer/VideoPlayer.vue'
import VideoDetails from './components/Videoplayer/VideoDetails.vue'
import Chat from './components/Chat/Chat.vue'

import {onMounted, ref} from 'vue'

let currentVideo = ref("");
let currentDescription = ref("");
let currentUsername = ref("");
let currentIndex = ref(0);
let videos = ref([]);

// onMountend, fetch date from the API and log it in the console
// api url = https://api.jsonbin.io/v3/b/670ccbfaad19ca34f8b81951



onMounted(() => {
  fetch('https://api.jsonbin.io/v3/b/670ccbfaad19ca34f8b81951')
    .then(response => response.json())
    .then(data => {
      videos.value = data.record.data.videos;
      console.log(data.record.data.videos);
      currentVideo.value = data.record.data.videos[0].video;
      currentDescription.value = data.record.data.videos[0].description;
      currentUsername.value = data.record.data.videos[0].username;
    })
});

function playNext(){
  //check the index, take the next one, if its is the last one, go back to the first one
  if (currentIndex.value < videos.value.length - 1) {
    currentIndex.value++;
  } else {
    currentIndex.value = 0;
  }
  currentVideo.value = videos.value[currentIndex.value].video;
  currentDescription.value = videos.value[currentIndex.value].description;
  currentUsername.value = videos.value[currentIndex.value].username;
  //check the length of the videos array, if it is the last one, go back to the first one

  
}

function playPrev(){
  //check the index, take the previous one, if its is the first one, go back to the last one
  if (currentIndex.value > 0) {
    currentIndex.value--;
  } else {
    currentIndex.value = videos.value.length - 1;
  }
  currentVideo.value = videos.value[currentIndex.value].video;
  currentDescription.value = videos.value[currentIndex.value].description;
  currentUsername.value = videos.value[currentIndex.value].username;
}



</script>

<template>
  <div class="tiktok">
  <div><VideoPlayer @play-next="playNext" @play-prev="playPrev" :src="currentVideo"/></div>
  <div>
    <VideoDetails :description="currentDescription" :username="currentUsername"/>
    <Chat />
  </div>
  </div>
</template>

<style scoped>

</style>
