<script setup lang="ts">
interface Faction {
  id: number
  name: string
  color: string
  territory: {
    points: [number, number][]
  }
}

const factions: Faction[] = [
  {
    id: 1,
    name: 'United Empire of Earth',
    color: '#4a90e2',
    territory: {
      points: [[20, 20], [80, 20], [80, 80], [20, 80]]
    }
  },
  {
    id: 2,
    name: 'Vanduul Space',
    color: '#e74c3c',
    territory: {
      points: [[85, 25], [95, 25], [95, 75], [85, 75]]
    }
  }
]

const createPath = (points: [number, number][]) => {
  return `M ${points.map(p => p.join(',')).join(' L ')} Z`
}
</script>

<template>
  <div class="faction-boundaries">
    <svg width="100%" height="100%" preserveAspectRatio="none">
      <g v-for="faction in factions" :key="faction.id">
        <path
          :d="createPath(faction.territory.points)"
          :fill="faction.color"
          fill-opacity="0.1"
          :stroke="faction.color"
          stroke-opacity="0.3"
          stroke-width="2"
          vector-effect="non-scaling-stroke"
        />
        <text
          :x="faction.territory.points[0][0] + 5"
          :y="faction.territory.points[0][1] + 20"
          :fill="faction.color"
          font-size="12"
          opacity="0.8"
        >
          {{ faction.name }}
        </text>
      </g>
    </svg>
  </div>
</template>

<style scoped>
.faction-boundaries {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
}

svg {
  width: 100%;
  height: 100%;
}

text {
  text-shadow: 0 0 4px rgba(0, 0, 0, 0.5);
}
</style>