<script setup lang="ts">
// TypeScript Interface: Definiert die Struktur der Props
// Props = Daten die von außen reinkommen
interface Props {
  name: string        // Text
  role: string        // Text
  status: string      // Text wie "Available" oder "Busy"
  statusColor: string // Farbe für den Status-Dot
}

// defineProps sagt Vue: "Diese Komponente erwartet diese Daten"
const props = defineProps<Props>()
</script>

<template>
  <div class="profile-card">
    <!-- Card Header -->
    <div class="card-header">
      <div class="status-indicator">
        <!-- :style bindet CSS dynamisch -->
        <span 
          class="status-dot" 
          :style="{ backgroundColor: props.statusColor }"
        ></span>
        <span class="status-text">{{ props.status }}</span>
      </div>
    </div>

    <!-- Card Body -->
    <div class="card-body">
      <div class="profile-icon">👨‍💻</div>
      <h2 class="profile-name">{{ props.name }}</h2>
      <p class="profile-role">{{ props.role }}</p>
    </div>

    <!-- Card Footer mit Fake Terminal-Ausgabe -->
    <div class="card-footer">
      <code class="terminal-line">
        <span class="prompt">$</span> 
        <span class="cmd">echo $ROLE</span>
      </code>
      <code class="terminal-output">{{ props.role }}</code>
    </div>
  </div>
</template>

<style scoped>
.profile-card {
  background: var(--terminal-bg, #161b22);
  border: 1px solid #30363d;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.2s, box-shadow 0.2s;
}

.profile-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.4);
}

/* Header mit Status */
.card-header {
  background: #21262d;
  padding: 0.75rem 1rem;
  border-bottom: 1px solid #30363d;
}

.status-indicator {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.85rem;
  color: #8b949e;
}

.status-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  /* Farbe kommt von props.statusColor */
}

/* Body mit Profil-Info */
.card-body {
  padding: 2rem;
  text-align: center;
}

.profile-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.profile-name {
  font-size: 1.5rem;
  color: #c9d1d9;
  margin: 0.5rem 0;
  font-weight: 600;
}

.profile-role {
  color: #58a6ff;
  font-size: 1rem;
  margin: 0;
  font-family: 'Courier New', monospace;
}

/* Footer mit Terminal-Look */
.card-footer {
  background: #0d1117;
  padding: 1rem;
  font-family: 'Courier New', monospace;
  font-size: 0.9rem;
  border-top: 1px solid #30363d;
}

.terminal-line {
  display: block;
  color: #c9d1d9;
  margin-bottom: 0.25rem;
}

.prompt {
  color: #58a6ff;
  font-weight: bold;
}

.cmd {
  color: #7ee787;
  margin-left: 0.5rem;
}

.terminal-output {
  display: block;
  color: #f0883e;
  margin-top: 0.25rem;
}
</style>