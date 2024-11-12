<script setup lang="ts">
interface JumpPoint {
  id: number
  name: string
  source: {
    systemId: number
    x: number
    y: number
  }
  destination: {
    systemId: number
    x: number
    y: number
  }
  status: 'stable' | 'unstable' | 'restricted'
}

const jumpPoints: JumpPoint[] = [
  {
    id: 1,
    name: 'Terra Jump Point',
    source: {
      systemId: 1,
      x: 75,
      y: 45
    },
    destination: {
      systemId: 2,
      x: 25,
      y: 65
    },
    status: 'stable'
  }
]

const getJumpPointColor = (status: JumpPoint['status']) => {
  switch (status) {
    case 'stable': return '#52ecff'
    case 'unstable': return '#ffc652'
    case 'restricted': return '#ff5252'
    default: return '#52ecff'
  }
}
</script>

<template>
  <div class="jump-points">
    <svg width="100%" height="100%" preserveAspectRatio="none">
      <defs>
        <marker
          id="arrowhead"
          markerWidth="10"
          markerHeight="7"
          refX="9"
          refY="3.5"
          orient="auto"
        >
          <polygon points="0 0, 10 3.5, 0 7" fill="#52ecff" />
        </marker>
      </defs>
      
      <g v-for="point in jumpPoints" :key="point.id">
        <line
          :x1="`${point.source.x}%`"
          :y1="`${point.source.y}%`"
          :x2="`${point.destination.x}%`"
          :y2="`${point.destination.y}%`"
          :stroke="getJumpPointColor(point.status)"
          stroke-width="2"
          stroke-dasharray="4 2"
          marker-end="url(#arrowhead)"
        />
        <circle
          :cx="`${point.source.x}%`"
          :cy="`${point.source.y}%`"
          r="4"
          :fill="getJumpPointColor(point.status)"
        />
        <text
          :x="`${(point.source.x + point.destination.x) / 2}%`"
          :y="`${(point.source.y + point.destination.y) / 2 - 10}%`"
          :fill="getJumpPointColor(point.status)"
          font-size="12"
          text-anchor="middle"
        >
          {{ point.name }}
        </text>
      </g>
    </svg>
  </div>
</template>

<style scoped>
.jump-points {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
  opacity: 0;
}

svg {
  width: 100%;
  height: 100%;
}

text {
  text-shadow: 0 0 4px rgba(0, 0, 0, 0.5);
}
</style>