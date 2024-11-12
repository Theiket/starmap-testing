<script setup lang="ts">
interface StarSystemProps {
  id: number
  name: string
  x: number
  y: number
  type: 'habitable' | 'mining' | 'dangerous'
  status: 'explored' | 'unexplored'
  size: number
  population: string
  affiliation: string
  risk?: 'low' | 'medium' | 'high'
  resources?: string[]
  jumpPoints?: number
}

const props = defineProps<StarSystemProps>()

const emit = defineEmits<{
  (e: 'select', id: number): void
}>()

const handleClick = () => {
  emit('select', props.id)
}

const getSystemColor = () => {
  switch (props.type) {
    case 'habitable': return '#52ff82'
    case 'mining': return '#ffc652'
    case 'dangerous': return '#ff5252'
    default: return '#52ecff'
  }
}

const getRiskLevel = () => {
  if (!props.risk) return ''
  return `Risk Level: ${props.risk.charAt(0).toUpperCase() + props.risk.slice(1)}`
}
</script>

<template>
  <div 
    class="star-system" 
    :class="[type, status]"
    :style="{ 
      left: `${x}%`, 
      top: `${y}%`,
      '--system-size': `${size}px`,
      '--system-color': getSystemColor()
    }"
    @click="handleClick"
  >
    <div class="orbital-lines"></div>
    <div class="pulse"></div>
    <div class="core"></div>
    <div class="info">
      <h3>{{ name }}</h3>
      <div class="details">
        <span class="type">{{ type }}</span>
        <span class="affiliation">{{ affiliation }}</span>
        <span v-if="risk" class="risk" :class="risk">{{ getRiskLevel() }}</span>
        <span v-if="jumpPoints" class="jump-points">Jump Points: {{ jumpPoints }}</span>
        <div v-if="resources && resources.length" class="resources">
          <span>Resources:</span>
          <span v-for="resource in resources" :key="resource" class="resource-tag">
            {{ resource }}
          </span>
        </div>
      </div>
    </div>
    <div v-if="jumpPoints" class="jump-connections"></div>
  </div>
</template>

<style scoped>
.star-system {
  position: absolute;
  width: var(--system-size);
  height: var(--system-size);
  cursor: pointer;
  transform: translate(-50%, -50%);
  z-index: 1;
}

.core {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: radial-gradient(circle at center, var(--system-color), rgba(255, 255, 255, 0.3));
  box-shadow: 0 0 20px var(--system-color);
  transition: all 0.3s ease;
}

.orbital-lines {
  position: absolute;
  width: 200%;
  height: 200%;
  top: -50%;
  left: -50%;
  border: 1px solid rgba(82, 236, 255, 0.2);
  border-radius: 50%;
  animation: rotate 20s linear infinite;
  pointer-events: none;
}

.orbital-lines::before,
.orbital-lines::after {
  content: '';
  position: absolute;
  width: 80%;
  height: 80%;
  top: 10%;
  left: 10%;
  border: 1px solid rgba(82, 236, 255, 0.1);
  border-radius: 50%;
}

.pulse {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  animation: pulse 2s infinite;
  background: var(--system-color);
  opacity: 0.3;
  pointer-events: none;
}

.info {
  position: absolute;
  left: calc(100% + 10px);
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 12, 24, 0.95);
  padding: 12px;
  border-radius: 4px;
  border: 1px solid var(--system-color);
  opacity: 0;
  transition: all 0.3s;
  pointer-events: none;
  min-width: 200px;
  z-index: 2;
  backdrop-filter: blur(4px);
}

.details {
  display: flex;
  flex-direction: column;
  gap: 6px;
  font-size: 0.8em;
  margin-top: 8px;
}

.star-system:hover .info {
  opacity: 1;
  transform: translateY(-50%) translateX(5px);
}

.star-system:hover .core {
  transform: scale(1.2);
}

.risk {
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 0.9em;
}

.risk.low { background: rgba(82, 255, 130, 0.2); color: #52ff82; }
.risk.medium { background: rgba(255, 198, 82, 0.2); color: #ffc652; }
.risk.high { background: rgba(255, 82, 82, 0.2); color: #ff5252; }

.resources {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
  margin-top: 4px;
}

.resource-tag {
  background: rgba(82, 236, 255, 0.1);
  padding: 2px 6px;
  border-radius: 3px;
  font-size: 0.9em;
}

.jump-connections {
  position: absolute;
  width: 200%;
  height: 200%;
  top: -50%;
  left: -50%;
  border: 1px dashed rgba(82, 236, 255, 0.3);
  border-radius: 50%;
  animation: pulse 4s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 0.8;
  }
  100% {
    transform: scale(2.5);
    opacity: 0;
  }
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>