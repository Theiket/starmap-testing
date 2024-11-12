<script setup lang="ts">
import { ref, onMounted } from 'vue'
import Starmap from './components/Starmap.vue'
import LoadingScreen from './components/LoadingScreen.vue'

const showStarmap = ref(false)

onMounted(() => {
  setTimeout(() => {
    showStarmap.value = true
  }, 5000)
})
</script>

<template>
  <LoadingScreen />
  <Transition name="fade">
    <Starmap v-if="showStarmap" />
  </Transition>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  background: #000813;
  color: #52ecff;
  font-family: 'Arial', sans-serif;
}

#app {
  width: 100vw;
  height: 100vh;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>