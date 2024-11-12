<script setup lang="ts">
defineEmits<{
  (e: 'click'): void
}>()
</script>

<template>
  <div class="star-container" @click="$emit('click')">
    <div class="star-corona"></div>
    <div class="star-core"></div>
    <div class="star-flare"></div>
  </div>
</template>

<style scoped>
.star-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100px;
  height: 100px;
  cursor: pointer;
  z-index: 100;
}

.star-core {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 30px;
  height: 30px;
  background: radial-gradient(
    circle at center,
    rgba(255, 255, 255, 1) 0%,
    rgba(255, 215, 0, 0.8) 40%,
    rgba(255, 140, 0, 0.4) 80%
  );
  border-radius: 50%;
  box-shadow: 
    0 0 60px rgba(255, 215, 0, 0.4),
    0 0 120px rgba(255, 140, 0, 0.2);
  animation: pulse 4s ease-in-out infinite;
}

.star-corona {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: radial-gradient(
    circle at center,
    rgba(255, 140, 0, 0.2) 0%,
    rgba(255, 140, 0, 0) 70%
  );
  animation: rotate 20s linear infinite;
}

.star-flare {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 45px;
  height: 45px;
  background: 
    radial-gradient(2px 100px at 50% 50%, rgba(255, 215, 0, 0.3) 0%, transparent 100%),
    radial-gradient(100px 2px at 50% 50%, rgba(255, 215, 0, 0.3) 0%, transparent 100%);
  animation: rotate 100s linear infinite;
  opacity:0;
}

@keyframes pulse {
  0%, 100% { transform: translate(-50%, -50%) scale(1); }
  50% { transform: translate(-50%, -50%) scale(1.1); }
}

@keyframes rotate {
  from { transform: translate(-50%, -50%) rotate(0deg); }
  to { transform: translate(-50%, -50%) rotate(360deg); }
}
</style>