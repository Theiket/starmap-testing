<script setup lang="ts">
import { ref, computed } from 'vue'
import StarSystem from './StarSystem.vue'
import SystemDetails from './SystemDetails.vue'
import SystemObjects from './SystemObjects.vue'

interface System {
  id: number
  name: string
  x: number
  y: number
  type: 'habitable' | 'mining' | 'dangerous'
  status: 'explored' | 'unexplored'
  size: number
  population: string
  affiliation: string
  description: string
  risk?: 'low' | 'medium' | 'high'
  resources?: string[]
  jumpPoints?: number
}

const systems = ref<System[]>([
  {
    id: 1,
    name: 'Stanton',
    x: 25,
    y: 30,
    type: 'habitable',
    status: 'explored',
    size: 24,
    population: 'Billions',
    affiliation: 'United Empire of Earth',
    description: 'A highly developed star system with four super-Earths, each terraformed and owned by major corporations.',
    risk: 'low',
    resources: ['Minerals', 'Technology', 'Agriculture'],
    jumpPoints: 4
  },
  {
    id: 2,
    name: 'Pyro',
    x: 45,
    y: 55,
    type: 'dangerous',
    status: 'unexplored',
    size: 20,
    population: 'Minimal',
    affiliation: 'Unclaimed',
    description: 'A red dwarf system known for its dangerous conditions and lawless inhabitants.',
    risk: 'high',
    resources: ['Salvage', 'Contraband'],
    jumpPoints: 2
  },
  {
    id: 3,
    name: 'Nyx',
    x: 65,
    y: 35,
    type: 'mining',
    status: 'explored',
    size: 18,
    population: 'Moderate',
    affiliation: 'People\'s Alliance',
    description: 'Rich in minerals, this system has become a major mining hub despite its remote location.',
    risk: 'medium',
    resources: ['Titanium', 'Gold', 'Diamonds'],
    jumpPoints: 3
  },
  {
    id: 4,
    name: 'Terra',
    x: 75,
    y: 70,
    type: 'habitable',
    status: 'explored',
    size: 28,
    population: 'Dense',
    affiliation: 'United Empire of Earth',
    description: 'The cultural capital of humanity, featuring Earth-like conditions and advanced civilization.',
    risk: 'low',
    resources: ['Technology', 'Culture', 'Commerce'],
    jumpPoints: 5
  },
  {
    id: 5,
    name: 'Odin',
    x: 15,
    y: 65,
    type: 'mining',
    status: 'unexplored',
    size: 16,
    population: 'Sparse',
    affiliation: 'Independent',
    description: 'A system rich in rare minerals, currently being explored by independent mining operations.',
    risk: 'medium',
    resources: ['Rare Earth', 'Platinum', 'Uranium'],
    jumpPoints: 2
  },
])

const selectedSystem = ref<System | null>(null)
const zoom = ref(1)
const isSystemView = ref(false)

const handleZoom = (delta: number) => {
  if (!isSystemView.value) {
    const newZoom = zoom.value + delta * 0.1
    zoom.value = Math.max(0.5, Math.min(2, newZoom))
  }
}

const handleSystemSelect = (systemId: number) => {
  const system = systems.value.find(s => s.id === systemId)
  if (system) {
    selectedSystem.value = system
    isSystemView.value = true
    zoom.value = 2
  }
}

const handleBackToGalaxy = () => {
  isSystemView.value = false
  selectedSystem.value = null
  zoom.value = 1
}

const mapStyle = computed(() => ({
  transform: `scale(${zoom.value})`
}))

const generateStars = () => {
  const stars = []
  for (let i = 0; i < 200; i++) {
    const x = Math.random() * 100
    const y = Math.random() * 100
    const size = Math.random() * 2 + 1
    const opacity = Math.random() * 0.8 + 0.2
    const delay = Math.random() * 4
    const duration = 2 + Math.random() * 4
    stars.push({ x, y, size, opacity, delay, duration })
  }
  return stars
}

const backgroundStars = generateStars()
</script>

<template>
  <div 
    class="starmap-container"
    @wheel="handleZoom($event.deltaY > 0 ? -1 : 1)"
  >
    <div class="background-stars">
      <div 
        v-for="(star, index) in backgroundStars" 
        :key="index"
        class="background-star"
        :style="{
          left: `${star.x}%`,
          top: `${star.y}%`,
          width: `${star.size}px`,
          height: `${star.size}px`,
          opacity: star.opacity,
          animationDelay: `${star.delay}s`,
          animationDuration: `${star.duration}s`
        }"
      ></div>
    </div>
    <div class="starmap" :style="mapStyle" :class="{ 'system-view': isSystemView }">
      <div class="grid"></div>
      <StarSystem
        v-for="system in systems"
        :key="system.id"
        v-bind="system"
        @select="handleSystemSelect"
        :class="{ 
          hidden: isSystemView && system.id !== selectedSystem?.id,
          'selected': system.id === selectedSystem?.id
        }"
      />
      <SystemObjects
        v-if="selectedSystem"
        :systemId="selectedSystem.id"
        :isVisible="isSystemView"
        @click-star="handleBackToGalaxy"
      />
    </div>
    <SystemDetails
      v-if="selectedSystem"
      :system="selectedSystem"
      class="details-panel"
    />
    <div class="controls">
      <button v-if="isSystemView" @click="handleBackToGalaxy" class="back-button">
        Back to Galaxy
      </button>
      <button @click="handleZoom(1)" :disabled="isSystemView">+</button>
      <button @click="handleZoom(-1)" :disabled="isSystemView">-</button>
    </div>
  </div>
</template>

<style scoped>
.starmap-container {
  width: 100%;
  height: 100vh;
  background: #000813;
  position: relative;
  overflow: hidden;
}

.background-stars {
  position: absolute;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.background-star {
  position: absolute;
  background: #fff;
  border-radius: 50%;
  animation: twinkle infinite ease-in-out;
}

.starmap {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  perspective: 1000px;
  transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.starmap.system-view {
  transform: scale(2.5) !important;
}

.grid {
  position: absolute;
  width: 200%;
  height: 200%;
  top: -50%;
  left: -50%;
  background-image: 
    linear-gradient(rgba(82, 236, 255, 0.1) 1px, transparent 1px),
    linear-gradient(90deg, rgba(82, 236, 255, 0.1) 1px, transparent 1px);
  background-size: 50px 50px;
  transform: rotateX(60deg);
  animation: grid-move 60s linear infinite;
  pointer-events: none;
}

.details-panel {
  position: absolute;
  right: 20px;
  top: 20px;
  width: 300px;
  z-index: 3;
}

.controls {
  position: absolute;
  right: 20px;
  bottom: 20px;
  display: flex;
  gap: 10px;
  z-index: 3;
}

.controls button {
  width: 40px;
  height: 40px;
  border: 1px solid rgba(82, 236, 255, 0.3);
  background: rgba(0, 12, 24, 0.8);
  color: #52ecff;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1.2em;
  transition: all 0.3s ease;
}

.controls button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.controls button:not(:disabled):hover {
  background: rgba(82, 236, 255, 0.2);
}

.back-button {
  width: auto !important;
  padding: 0 20px;
}

.hidden {
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.5s ease;
}

.selected {
  z-index: 2;
}

@keyframes grid-move {
  from {
    transform: rotateX(60deg) translateY(0);
  }
  to {
    transform: rotateX(60deg) translateY(50px);
  }
}

@keyframes twinkle {
  0%, 100% {
    opacity: 0.2;
  }
  50% {
    opacity: 1;
  }
}
</style>