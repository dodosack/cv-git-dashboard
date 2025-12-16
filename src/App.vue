<script setup lang="ts">
import { ref } from 'vue'

// Reaktive Variable für Dark Mode
// ref() macht eine Variable "reaktiv" - Vue aktualisiert die Ansicht automatisch
const isDarkMode = ref(true)

// Funktion zum Umschalten
const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value
}
</script>

<template>
  <!-- :class bindet CSS-Klassen dynamisch -->
  <!-- Wenn isDarkMode true ist, wird 'dark' als Klasse hinzugefügt -->
  <div :class="{ dark: isDarkMode }" class="app-container">

    <!-- Header mit Toggle-Button -->
    <header class="dashboard-header">
      <div class="header-content">
        <h1>// DEVELOPER.DASHBOARD</h1>
        <button @click="toggleTheme" class="theme-toggle">
          {{ isDarkMode ? '☀️' : '🌙' }}
        </button>
      </div>
    </header>

    <!-- Main Content Area -->
    <main class="dashboard-main">
      <div class="terminal-window">
        <div class="terminal-header">
          <span class="terminal-button red"></span>
          <span class="terminal-button yellow"></span>
          <span class="terminal-button green"></span>
          <span class="terminal-title">terminal</span>
        </div>
        <div class="terminal-body">
          <p class="terminal-line">
            <span class="prompt">$</span>
            <span class="command">whoami</span>
          </p>
          <p class="terminal-output">Dein Name hier</p>
          <p class="terminal-line">
            <span class="prompt">$</span>
            <span class="command">cat skills.txt</span>
          </p>
          <p class="terminal-output">Vue.js | TypeScript | Git</p>
        </div>
      </div>
    </main>

  </div>
</template>

<style scoped>
/* Root Variablen für Farben - wie Einstellungen */
.app-container {
  --bg-color: #ffffff;
  --text-color: #333333;
  --terminal-bg: #f5f5f5;
  --terminal-text: #000000;
  min-height: 100vh;
  background: var(--bg-color);
  color: var(--text-color);
  transition: all 0.3s ease;
}

/* Dark Mode Farben */
.app-container.dark {
  --bg-color: #0d1117;
  --text-color: #c9d1d9;
  --terminal-bg: #161b22;
  --terminal-text: #58a6ff;
}

/* Header Styling */
.dashboard-header {
  background: var(--terminal-bg);
  border-bottom: 1px solid #30363d;
  padding: 1rem 2rem;
}

.header-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.dashboard-header h1 {
  font-family: 'Courier New', monospace;
  font-size: 1.5rem;
  color: var(--terminal-text);
  margin: 0;
}

.theme-toggle {
  background: transparent;
  border: 2px solid var(--terminal-text);
  color: var(--terminal-text);
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1.2rem;
  transition: all 0.2s;
}

.theme-toggle:hover {
  background: var(--terminal-text);
  color: var(--bg-color);
}

/* Main Content */
.dashboard-main {
  max-width: 1200px;
  margin: 2rem auto;
  padding: 0 2rem;
}

/* Terminal Window */
.terminal-window {
  background: var(--terminal-bg);
  border: 1px solid #30363d;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
}

.terminal-header {
  background: #21262d;
  padding: 0.5rem 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.terminal-button {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
}

.terminal-button.red { background: #ff5f56; }
.terminal-button.yellow { background: #ffbd2e; }
.terminal-button.green { background: #27c93f; }

.terminal-title {
  margin-left: auto;
  color: #8b949e;
  font-size: 0.9rem;
  font-family: monospace;
}

.terminal-body {
  padding: 1.5rem;
  font-family: 'Courier New', monospace;
  font-size: 1rem;
  line-height: 1.6;
}

.terminal-line {
  margin: 0.5rem 0;
}

.prompt {
  color: #58a6ff;
  font-weight: bold;
}

.command {
  color: #7ee787;
  margin-left: 0.5rem;
}

.terminal-output {
  color: var(--text-color);
  margin: 0.25rem 0 1rem 1.5rem;
}
</style>
