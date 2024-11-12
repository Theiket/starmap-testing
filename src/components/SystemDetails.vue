<script setup lang="ts">
interface System {
  id: number
  name: string
  type: string
  status: string
  population: string
  affiliation: string
  description: string
}

defineProps<{
  system: System
}>()
</script>

<template>
  <div class="system-details">
    <div class="header">
      <h2>{{ system.name }}</h2>
      <div class="status-badge" :class="system.status">
        {{ system.status }}
      </div>
    </div>
    
    <div class="info-grid">
      <div class="info-item">
        <span class="label">Type</span>
        <span class="value">{{ system.type }}</span>
      </div>
      <div class="info-item">
        <span class="label">Population</span>
        <span class="value">{{ system.population }}</span>
      </div>
      <div class="info-item">
        <span class="label">Affiliation</span>
        <span class="value">{{ system.affiliation }}</span>
      </div>
    </div>

    <p class="description">{{ system.description }}</p>
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
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.status-badge {
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.8em;
  text-transform: uppercase;
}

.status-badge.explored {
  background: rgba(82, 255, 168, 0.2);
  color: rgb(82, 255, 168);
}

.status-badge.unexplored {
  background: rgba(255, 82, 82, 0.2);
  color: rgb(255, 82, 82);
}

.info-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
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
}
</style>