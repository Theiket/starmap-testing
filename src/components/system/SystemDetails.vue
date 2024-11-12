<script setup lang="ts">
interface CelestialObject {
  id: number
  name: string
  type: string
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
}

defineProps<{
  object: CelestialObject
}>()

const getDangerLevel = (level: number) => {
  const levels = ['Safe', 'Low', 'Medium', 'High', 'Extreme']
  return levels[level - 1] || 'Unknown'
}
</script>

<template>
  <div class="system-details">
    <div class="header">
      <h2>{{ object.name }}</h2>
      <div v-if="object.danger" class="danger-badge" :data-level="object.danger">
        {{ getDangerLevel(object.danger) }}
      </div>
    </div>
    
    <div class="info-grid">
      <template v-if="object.type === 'planet'">
        <div class="info-item">
          <span class="label">Climate</span>
          <span class="value">{{ object.climate }}</span>
        </div>
        <div class="info-item">
          <span class="label">Gravity</span>
          <span class="value">{{ object.gravity }}</span>
        </div>
      </template>
      <div class="info-item" v-if="object.population">
        <span class="label">Population</span>
        <span class="value">{{ object.population }}</span>
      </div>
      <div class="info-item" v-if="object.economy">
        <span class="label">Economy</span>
        <span class="value">{{ object.economy }}</span>
      </div>
      <div class="info-item" v-if="object.controlledBy">
        <span class="label">Control</span>
        <span class="value">{{ object.controlledBy }}</span>
      </div>
    </div>

    <p class="description">{{ object.description }}</p>

    <!-- Surface Information -->
    <template v-if="object.surfaceInfo">
      <div class="surface-info">
        <h3>Surface Analysis</h3>
        
        <div class="surface-stats">
          <div class="stat">
            <span class="label">Atmosphere</span>
            <span class="value">{{ object.surfaceInfo.atmosphere }}</span>
          </div>
          <div class="stat">
            <span class="label">Temperature</span>
            <span class="value">{{ object.surfaceInfo.temperature }}</span>
          </div>
          <div class="stat">
            <span class="label">Water Coverage</span>
            <span class="value">{{ object.surfaceInfo.waterCoverage }}%</span>
          </div>
        </div>

        <div class="biomes">
          <h4>Biomes</h4>
          <div class="biome-distribution">
            <div 
              v-for="biome in object.surfaceInfo.biomes" 
              :key="biome.name"
              class="biome"
              :style="{ width: `${biome.coverage}%` }"
              :title="`${biome.name} (${biome.coverage}%)`"
            >
              <div class="biome-info">
                <span class="name">{{ biome.name }}</span>
                <span class="coverage">{{ biome.coverage }}%</span>
              </div>
            </div>
          </div>
          <div class="resources">
            <h4>Available Resources</h4>
            <div class="resource-tags">
              <span 
                v-for="resource in biome.resources"
                :key="resource"
                class="resource-tag"
              >
                {{ resource }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </template>
  </div>
</template>

<style scoped>
.system-details {
  padding: 20px;
  background: rgba(0, 12, 24, 0.95);
  border: 1px solid rgba(82, 236, 255, 0.3);
  border-radius: 8px;
  color: #fff;
  backdrop-filter: blur(10px);
  max-width: 400px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.danger-badge {
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.8em;
  text-transform: uppercase;
}

.danger-badge[data-level="1"] { background: rgba(82, 255, 168, 0.2); color: rgb(82, 255, 168); }
.danger-badge[data-level="2"] { background: rgba(255, 198, 82, 0.2); color: rgb(255, 198, 82); }
.danger-badge[data-level="3"] { background: rgba(255, 140, 0, 0.2); color: rgb(255, 140, 0); }
.danger-badge[data-level="4"] { background: rgba(255, 82, 82, 0.2); color: rgb(255, 82, 82); }
.danger-badge[data-level="5"] { background: rgba(255, 0, 0, 0.3); color: rgb(255, 0, 0); }

.info-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  margin-bottom: 20px;
}

.info-item {
  display: flex;
  flex-direction: column;
}

.label {
  font-size: 0.8em;
  color: rgba(255, 255, 255, 0.6);
  margin-bottom: 4px;
}

.value {
  color: #52ecff;
}

.description {
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
  margin-bottom: 20px;
}

.surface-info {
  border-top: 1px solid rgba(82, 236, 255, 0.2);
  padding-top: 20px;
  margin-top: 20px;
}

.surface-info h3 {
  color: #52ecff;
  margin-bottom: 15px;
}

.surface-stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-bottom: 20px;
}

.stat {
  text-align: center;
  padding: 10px;
  background: rgba(82, 236, 255, 0.1);
  border-radius: 4px;
}

.biomes {
  margin-top: 20px;
}

.biome-distribution {
  display: flex;
  height: 30px;
  border-radius: 4px;
  overflow: hidden;
  margin: 10px 0;
}

.biome {
  position: relative;
  height: 100%;
  transition: all 0.3s ease;
}

.biome:nth-child(1) { background: #2ecc71; }
.biome:nth-child(2) { background: #3498db; }
.biome:nth-child(3) { background: #f1c40f; }
.biome:nth-child(4) { background: #e74c3c; }
.biome:nth-child(5) { background: #9b59b6; }

.biome-info {
  position: absolute;
  top: -30px;
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
  font-size: 0.8em;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.biome:hover .biome-info {
  opacity: 1;
}

.resources {
  margin-top: 20px;
}

.resource-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-top: 10px;
}

.resource-tag {
  background: rgba(82, 236, 255, 0.1);
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.8em;
  color: #52ecff;
}
</style>