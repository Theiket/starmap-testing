<script setup lang="ts">
interface Props {
  id: number
  name: string
  type: 'planet' | 'moon' | 'station' | 'jumppoint' | 'asteroid-belt'
  orbitRadius: number
  orbitSpeed: number
  size: number
  selected?: boolean
  moons?: Array<any>
}

const props = defineProps<Props>()

const emit = defineEmits<{
  (e: 'select', object: Props): void
}>()

const handleClick = () => {
  emit('select', props)
}

const getOrbitColor = (type: Props['type']) => {
  switch (type) {
    case 'planet': return 'rgba(82, 236, 255, 0.15)'
    case 'moon': return 'rgba(255, 255, 255, 0.1)'
    case 'station': return 'rgba(255, 82, 82, 0.15)'
    case 'asteroid-belt': return 'rgba(255, 198, 82, 0.15)'
    default: return 'rgba(82, 236, 255, 0.1)'
  }
}
</script>

<template>
  <div 
    class="orbit"
    :class="type"
    :style="{ 
      width: orbitRadius * 2 + 'px',
      height: orbitRadius * 2 + 'px',
      borderColor: getOrbitColor(type)
    }"
  >
    <div 
      class="object"
      :class="[type, { selected }]"
      :style="{ 
        width: size + 'px',
        height: size + 'px'
      }"
      @click="handleClick"
    >
      <div class="object-info">
        <span class="name">{{ name }}</span>
        <span class="type">{{ type }}</span>
      </div>

      <template v-if="moons">
        <div 
          v-for="moon in moons"
          :key="moon.id"
          class="moon-orbit"
          :style="{
            width: moon.orbitRadius * 2 + 'px',
            height: moon.orbitRadius * 2 + 'px',
            borderColor: getOrbitColor('moon')
          }"
        >
          <div 
            class="object moon"
            :style="{
              width: moon.size + 'px',
              height: moon.size + 'px'
            }"
            @click.stop="$emit('select', moon)"
          >
            <div class="object-info">
              <span class="name">{{ moon.name }}</span>
              <span class="type">moon</span>
            </div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
.orbit {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border: 1px solid;
  border-radius: 50%;
  pointer-events: none;
  /*animation: orbit var(--orbit-speed, 20s) linear infinite;*/
}

.orbit.asteroid-belt {
  border-style: dashed;
  border-width: 2px;
  --orbit-speed: 30s;
}

.object {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  filter: brightness(0.8);
}

.object:hover {
  transform: translate(-50%, -50%) scale(1.2);
  filter: brightness(1.2);
}

.object.selected {
  transform: translate(-50%, -50%) scale(1.3);
  filter: brightness(1.4);
  z-index: 10;
}

.object.planet {
  background: radial-gradient(
    circle at 30% 30%,
    rgba(74, 144, 226, 1) 0%,
    rgba(44, 62, 80, 1) 100%
  );
  box-shadow: 
    inset -4px -4px 20px rgba(0, 0, 0, 0.5),
    0 0 20px rgba(74, 144, 226, 0.4);
}

.object.moon {
  background: radial-gradient(
    circle at 30% 30%,
    rgba(149, 165, 166, 1) 0%,
    rgba(127, 140, 141, 1) 100%
  );
  box-shadow: 
    inset -2px -2px 10px rgba(0, 0, 0, 0.5),
    0 0 10px rgba(149, 165, 166, 0.3);
}

.object.station {
  background: radial-gradient(
    circle at 30% 30%,
    rgba(231, 76, 60, 1) 0%,
    rgba(192, 57, 43, 1) 100%
  );
  box-shadow: 0 0 20px rgba(231, 76, 60, 0.4);
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
  border-radius: 0;
}

.object-info {
  position: absolute;
  top: -40px;
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
  text-align: center;
  pointer-events: none;
  z-index: 10;
  background: rgba(0, 12, 24, 0.7);
  padding: 4px 8px;
  border-radius: 4px;
  border: 1px solid rgba(82, 236, 255, 0.2);
  backdrop-filter: blur(4px);
}

.name {
  display: block;
  font-size: 14px;
  color: #fff;
  text-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
  font-weight: 500;
}

.type {
  display: block;
  font-size: 11px;
  color: rgba(82, 236, 255, 0.8);
  text-transform: uppercase;
  letter-spacing: 1px;
}

@keyframes orbit {
  from { transform: translate(-50%, -50%) rotate(0deg); }
  to { transform: translate(-50%, -50%) rotate(360deg); }
}
</style>