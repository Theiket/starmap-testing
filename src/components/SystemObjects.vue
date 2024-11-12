<script setup lang="ts">
import { ref, computed } from 'vue'
import StarComponent from './system/StarComponent.vue'
import CelestialObject from './system/CelestialObject.vue'
import SystemDetails from './system/SystemDetails.vue'
import NavigationGrid from './system/NavigationGrid.vue'
import FactionBoundaries from './system/FactionBoundaries.vue'
import JumpPoints from './system/JumpPoints.vue'

interface CelestialObjectData {
  id: number
  name: string
  type: 'planet' | 'moon' | 'station' | 'jumppoint' | 'asteroid-belt'
  orbitRadius: number
  orbitSpeed: number
  size: number
  description: string
  climate?: string
  gravity?: string
  population?: string
  economy?: string
  controlledBy?: string
  danger?: number
  surfaceInfo?: {
    biomes: Array<{
      name: string
      coverage: number
      resources: string[]
    }>
    atmosphere: string
    waterCoverage: number
    temperature: string
  }
  moons?: CelestialObjectData[]
}

interface Props {
  systemId: number
  isVisible: boolean
}

const props = defineProps<Props>()
const selectedObject = ref<CelestialObjectData | null>(null)

const emit = defineEmits<{
  (e: 'click-star'): void
}>()

const systemObjects: Record<number, CelestialObjectData[]> = {
  1: [
    {
      id: 1,
      name: 'Hurston',
      type: 'planet',
      orbitRadius: 60,
      orbitSpeed: 20,
      size: 15,
      description: 'Corporate-owned planet with heavy industry',
      climate: 'Polluted Industrial',
      gravity: '1.3g',
      population: '3.5M',
      economy: 'Industrial/Mining',
      controlledBy: 'Hurston Dynamics',
      danger: 2,
      surfaceInfo: {
        biomes: [
          {
            name: 'Industrial Wasteland',
            coverage: 45,
            resources: ['Metals', 'Minerals', 'Industrial Waste']
          },
          {
            name: 'Toxic Plains',
            coverage: 30,
            resources: ['Rare Minerals', 'Toxic Materials']
          },
          {
            name: 'Reclaimed Forest',
            coverage: 15,
            resources: ['Timber', 'Medicinal Plants']
          },
          {
            name: 'Acid Lakes',
            coverage: 10,
            resources: ['Chemical Compounds']
          }
        ],
        atmosphere: 'Heavily Polluted',
        waterCoverage: 15,
        temperature: 'Hot (35°C avg)'
      },
      moons: [
        {
          id: 11,
          name: 'Arial',
          type: 'moon',
          orbitRadius: 15,
          orbitSpeed: 5,
          size: 5,
          description: 'Mining outpost moon',
          climate: 'Barren',
          gravity: '0.3g',
          surfaceInfo: {
            biomes: [
              {
                name: 'Rocky Plains',
                coverage: 70,
                resources: ['Iron', 'Nickel']
              },
              {
                name: 'Impact Craters',
                coverage: 30,
                resources: ['Rare Metals']
              }
            ],
            atmosphere: 'Thin',
            waterCoverage: 0,
            temperature: 'Cold (-50°C avg)'
          }
        }
      ]
    }
  ]
}

const getSystemObjects = computed(() => 
  systemObjects[props.systemId] || []
)

const handleStarClick = () => {
  emit('click-star')
}

const handleObjectClick = (object: CelestialObjectData) => {
  selectedObject.value = object
}

const systemStyle = computed(() => ({
  transform: `scale(${props.isVisible ? 1 : 0})`
}))
</script>

<template>
  <div class="system-objects" :class="{ visible: isVisible }">
    <NavigationGrid />
    <FactionBoundaries />
    <JumpPoints />
    
    <div class="system-view" :style="systemStyle">
      <StarComponent @click="handleStarClick" />
      
      <template v-for="object in getSystemObjects" :key="object.id">
        <CelestialObject
          v-bind="object"
          :selected="selectedObject?.id === object.id"
          @select="handleObjectClick"
        />
      </template>
    </div>

    <SystemDetails
      v-if="selectedObject"
      :object="selectedObject"
      class="details-panel"
    />
  </div>
</template>

<style scoped>
.system-objects {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.5s ease;
}

.system-objects.visible {
  opacity: 1;
  pointer-events: all;
}

.system-view {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.details-panel {
  position: absolute;
  right: 20px;
  top: 20px;
  z-index: 100;
}
</style>