<script setup lang="ts">
interface Props {
  name: string
  level: number  // 1-5
  category?: string  // Optional (das ? macht es optional)
}

const props = defineProps<Props>()

// Computed: Berechnet Farbe basierend auf Level
// Wird automatisch neu berechnet wenn level sich ändert
const getBadgeColor = () => {
  if (props.level >= 4) return '#7ee787'  // Grün für Expert
  if (props.level >= 3) return '#58a6ff'  // Blau für Intermediate
  return '#f0883e'  // Orange für Beginner
}
</script>

<template>
  <div class="skill-badge" :style="{ borderColor: getBadgeColor() }">
    <div class="skill-header">
      <span class="skill-name">{{ props.name }}</span>
      <span class="skill-category" v-if="props.category">
        {{ props.category }}
      </span>
    </div>
    <div class="skill-level">
      <!-- v-for: Loop von 1 bis 5 -->
      <span 
        v-for="n in 5" 
        :key="n"
        class="level-dot"
        :class="{ active: n <= props.level }"
      ></span>
    </div>
  </div>
</template>

<style scoped>
.skill-badge {
  background: #161b22;
  border: 2px solid #30363d;
  border-radius: 6px;
  padding: 1rem;
  transition: all 0.2s;
}

.skill-badge:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.skill-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}

.skill-name {
  font-weight: 600;
  color: #c9d1d9;
  font-size: 1rem;
}

.skill-category {
  font-size: 0.75rem;
  color: #8b949e;
  background: #21262d;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
}

.skill-level {
  display: flex;
  gap: 0.5rem;
}

.level-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: #21262d;
  border: 2px solid #30363d;
  transition: all 0.2s;
}

.level-dot.active {
  background: currentColor;
  border-color: currentColor;
}
</style>