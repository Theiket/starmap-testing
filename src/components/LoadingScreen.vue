<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isVisible = ref(true)

onMounted(() => {
  setTimeout(() => {
    isVisible.value = false
  }, 5000)
})
</script>

<template>
  <div class="loading-screen" :class="{ 'fade-out': !isVisible }">
    <div data-text="ATLAS" class="guide-text">ATLAS</div>
    <div class="scanning-line"></div>
  </div>
</template>

<style scoped>
.loading-screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: #2a2a2a;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  transition: opacity 1s ease-out;
}

.loading-screen.fade-out {
  opacity: 0;
  pointer-events: none;
}

.guide-text {
  font-size: 7rem;
  font-weight: bold;
  color: #2a2a2a;
  letter-spacing: 1rem;
  text-transform: uppercase;
  position: absolute;
  -webkit-text-stroke: 0.2vw #ff6b00;
  text-shadow: 0 0 10px rgba(255, 107, 0, 0);
  animation: pulse 6s infinite;
}
.guide-text::before {
  width: 0;
  height: 100%;
  color: #ff6b00;
  overflow: hidden;
  position: absolute;
  content: attr(data-text) / "";
  -webkit-text-stroke: 0vw #ff6b00;
  animation: animate 10s linear infinite;
}

.scanning-line {
  position: absolute;
  width: 100%;
  height: 2px;
  background: #ff6b00;
  animation: scan 5s ease-in-out infinite;
  opacity: 0%;
}

@keyframes pulse {
  0% {
    opacity: 0.5;
  }
  100% {
    opacity: 1;
  }
}

@keyframes animate {
  0%{
    width: 0;
  }
  
  50%,
  100%{
    width: 100%;
  }
}
  
@keyframes scan {
  0% {
    transform: translateY(-50vh);
  }
  50% {
    transform: translateY(50vh);
  }
  100% {
    transform: translateY(-50vh);
  }
}
</style>