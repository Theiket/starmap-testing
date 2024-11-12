<template>
  <div class="navigation-grid">
    <div class="grid-lines horizontal"></div>
    <div class="grid-lines vertical"></div>
    <div class="grid-overlay"></div>
  </div>
</template>

<style scoped>
.navigation-grid {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.grid-lines {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: 50px 50px;
  opacity: 0;
}

.horizontal {
  background-image: linear-gradient(0deg, #52ecff 1px, transparent 1px);
}

.vertical {
  background-image: linear-gradient(90deg, #52ecff 1px, transparent 1px);
}

.grid-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(
    circle at center,
    transparent 0%,
    rgba(0, 12, 24, 0.3) 100%
  );
}
</style>