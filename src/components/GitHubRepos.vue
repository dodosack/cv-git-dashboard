<script setup lang="ts">
import { ref, onMounted } from 'vue'

// TypeScript: Definiere die Struktur eines GitHub-Repos
// Wir nehmen nur die Daten die wir brauchen
interface GitHubRepo {
  id: number
  name: string
  description: string | null  // kann null sein
  html_url: string
  stargazers_count: number
  language: string | null
  updated_at: string
}

// Props: GitHub Username von außen
interface Props {
  username: string
  maxRepos?: number  // Optional: max. Anzahl Repos
}

const props = withDefaults(defineProps<Props>(), {
  maxRepos: 6
})

// Reaktive Variablen für Zustand
const repos = ref<GitHubRepo[]>([])       // Die Repo-Daten
const loading = ref(true)                  // Lädt gerade?
const error = ref<string | null>(null)     // Fehlermeldung

// Funktion: Repos von GitHub holen
const fetchRepos = async () => {
  try {
    // loading = true (wird schon oben gesetzt)
    error.value = null  // Fehler zurücksetzen
    
    // API-Aufruf
    const response = await fetch(
      `https://api.github.com/users/${props.username}/repos?sort=updated&per_page=${props.maxRepos}`
    )
    
    // Prüfen ob erfolgreich
    if (!response.ok) {
      throw new Error(`GitHub User "${props.username}" nicht gefunden`)
    }
    
    // JSON parsen (Text → JavaScript-Objekt)
    const data = await response.json()
    
    // Daten speichern
    repos.value = data
    
  } catch (e) {
    // Fehler abfangen
    error.value = e instanceof Error ? e.message : 'Unbekannter Fehler'
    console.error('GitHub API Fehler:', e)
  } finally {
    // loading = false (egal ob Erfolg oder Fehler)
    loading.value = false
  }
}

// onMounted: Wird ausgeführt wenn Komponente fertig geladen ist
// Hier laden wir die Daten
onMounted(() => {
  fetchRepos()
})

// Hilfsfunktion: Datum formatieren
const formatDate = (dateString: string) => {
  const date = new Date(dateString)
  return date.toLocaleDateString('de-DE', {
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  })
}
</script>

<template>
  <div class="github-repos">
    <h3 class="repos-title">// GITHUB_REPOS</h3>
    
    <!-- Loading State -->
    <div v-if="loading" class="loading-state">
      <div class="spinner"></div>
      <p>Lade Repositories...</p>
    </div>
    
    <!-- Error State -->
    <div v-else-if="error" class="error-state">
      <span class="error-icon">❌</span>
      <p>{{ error }}</p>
      <button @click="fetchRepos" class="retry-button">
        Erneut versuchen
      </button>
    </div>
    
    <!-- Success State: Repos anzeigen -->
    <div v-else class="repos-grid">
      
      <!-- v-for: Durch alle Repos loopen -->
      <div 
        v-for="repo in repos" 
        :key="repo.id"
        class="repo-card"
      >
        <!-- Repo Header -->
        <div class="repo-header">
          <h4 class="repo-name">{{ repo.name }}</h4>
          <div class="repo-stats">
            <span class="stars">⭐ {{ repo.stargazers_count }}</span>
          </div>
        </div>
        
        <!-- Repo Description -->
        <p class="repo-description">
          {{ repo.description || 'Keine Beschreibung' }}
        </p>
        
        <!-- Repo Footer -->
        <div class="repo-footer">
          <span v-if="repo.language" class="language">
            <span class="language-dot"></span>
            {{ repo.language }}
          </span>
          <span class="updated">
            {{ formatDate(repo.updated_at) }}
          </span>
        </div>
        
        <!-- Link zum Repo -->
        <a 
          :href="repo.html_url" 
          target="_blank"
          class="repo-link"
        >
          Zum Repository →
        </a>
      </div>
      
      <!-- Falls keine Repos -->
      <div v-if="repos.length === 0" class="no-repos">
        <p>Keine öffentlichen Repositories gefunden 🤷‍♂️</p>
      </div>
      
    </div>
  </div>
</template>

<style scoped>
.github-repos {
  margin-top: 2rem;
}

.repos-title {
  color: #58a6ff;
  font-family: 'Courier New', monospace;
  font-size: 1.3rem;
  margin: 0 0 1.5rem 0;
  padding-left: 0.5rem;
  border-left: 3px solid #58a6ff;
}

/* Loading State */
.loading-state {
  text-align: center;
  padding: 3rem;
  color: var(--text-color);
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #30363d;
  border-top-color: #58a6ff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Error State */
.error-state {
  text-align: center;
  padding: 3rem;
  color: #ff5f56;
  background: rgba(255, 95, 86, 0.1);
  border: 1px solid #ff5f56;
  border-radius: 8px;
}

.error-icon {
  font-size: 3rem;
  display: block;
  margin-bottom: 1rem;
}

.retry-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background: #ff5f56;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: opacity 0.2s;
}

.retry-button:hover {
  opacity: 0.8;
}

/* Repos Grid */
.repos-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}

/* Repo Card */
.repo-card {
  background: var(--terminal-bg, #161b22);
  border: 1px solid #30363d;
  border-radius: 8px;
  padding: 1.5rem;
  transition: all 0.2s;
  display: flex;
  flex-direction: column;
}

.repo-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
  border-color: #58a6ff;
}

.repo-header {
  display: flex;
  justify-content: space-between;
  align-items: start;
  margin-bottom: 0.75rem;
}

.repo-name {
  color: #58a6ff;
  font-size: 1.1rem;
  margin: 0;
  font-weight: 600;
  word-break: break-word;
}

.repo-stats {
  display: flex;
  gap: 0.5rem;
  font-size: 0.9rem;
}

.stars {
  color: #f0883e;
  white-space: nowrap;
}

.repo-description {
  color: var(--text-color, #8b949e);
  font-size: 0.9rem;
  line-height: 1.5;
  margin: 0 0 1rem 0;
  flex-grow: 1;
  min-height: 3rem;
}

.repo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.85rem;
  color: #8b949e;
  margin-bottom: 1rem;
  padding-top: 0.75rem;
  border-top: 1px solid #30363d;
}

.language {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.language-dot {
  width: 10px;
  height: 10px;
  background: #7ee787;
  border-radius: 50%;
}

.repo-link {
  color: #58a6ff;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s;
  text-align: center;
  padding: 0.5rem;
  border: 1px solid #58a6ff;
  border-radius: 4px;
}

.repo-link:hover {
  color: #ffffff;
  background: #58a6ff;
}

.no-repos {
  grid-column: 1 / -1;
  text-align: center;
  padding: 3rem;
  color: #8b949e;
  font-size: 1.1rem;
}
</style>