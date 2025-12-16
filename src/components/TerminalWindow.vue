<script setup lang="ts">
interface Props {
  title?: string  // Optional, default: "terminal"
}

const props = withDefaults(defineProps<Props>(), {
  title: 'terminal'
})
</script>

<template>
  <div class="terminal-window">
    <!-- Terminal Header mit Mac-Style Buttons -->
    <div class="terminal-header">
      <div class="terminal-buttons">
        <span class="terminal-button red"></span>
        <span class="terminal-button yellow"></span>
        <span class="terminal-button green"></span>
      </div>
      <span class="terminal-title">{{ props.title }}</span>
      <div class="terminal-spacer"></div>
    </div>

    <!-- Terminal Body - HIER kommt der Content von außen rein! -->
    <div class="terminal-body">
      <!-- <slot> ist der Platzhalter für Content -->
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.terminal-window {
  background: var(--terminal-bg, #161b22);
  border: 1px solid #30363d;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  transition: transform 0.2s;
}

.terminal-window:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
}

.terminal-header {
  background: #21262d;
  padding: 0.5rem 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  border-bottom: 1px solid #30363d;
}

.terminal-buttons {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

.terminal-button {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
  cursor: pointer;
  transition: opacity 0.2s;
}

.terminal-button:hover {
  opacity: 0.8;
}

.terminal-button.red { background: #ff5f56; }
.terminal-button.yellow { background: #ffbd2e; }
.terminal-button.green { background: #27c93f; }

.terminal-title {
  flex: 1;
  text-align: center;
  color: #8b949e;
  font-size: 0.9rem;
  font-family: monospace;
}

.terminal-spacer {
  width: 60px; /* Gleiche Breite wie Buttons für zentrierten Titel */
}

.terminal-body {
  padding: 1.5rem;
  font-family: 'Courier New', monospace;
  font-size: 1rem;
  line-height: 1.6;
  min-height: 100px;
}

/* Styles für Terminal-Content */
:deep(.terminal-line) {
  margin: 0.5rem 0;
  color: var(--text-color, #c9d1d9);
}

:deep(.prompt) {
  color: #58a6ff;
  font-weight: bold;
}

:deep(.command) {
  color: #7ee787;
  margin-left: 0.5rem;
}

:deep(.terminal-output) {
  color: var(--text-color, #c9d1d9);
  margin: 0.25rem 0 1rem 1.5rem;
}

:deep(.error) {
  color: #ff5f56;
}

:deep(.success) {
  color: #7ee787;
}

:deep(.comment) {
  color: #8b949e;
  font-style: italic;
}
</style>