<script setup lang="ts">
import { ref, onMounted } from 'vue'

// TypeScript Interface für einen News-Artikel
interface NewsArticle {
  title: string
  description: string | null
  url: string
  urlToImage: string | null
  publishedAt: string
  source: {
    name: string
  }
}

// Props
interface Props {
  category?: string  // z.B. "technology", "business"
  maxArticles?: number
}

const props = withDefaults(defineProps<Props>(), {
  category: 'technology',
  maxArticles: 4
})

// State
const articles = ref<NewsArticle[]>([])
const loading = ref(true)
const error = ref<string | null>(null)

// API-Key aus Environment Variable holen
const apiKey = import.meta.env.VITE_NEWS_API_KEY

// Funktion: News laden
const fetchNews = async () => {
  // Prüfen ob API-Key vorhanden
  if (!apiKey) {
    error.value = 'API-Key fehlt! Bitte .env.local Datei erstellen.'
    loading.value = false
    return
  }

  try {
    loading.value = true
    error.value = null

    // NewsAPI Endpoint
    const url = `https://newsapi.org/v2/top-headlines?category=${props.category}&language=en&pageSize=${props.maxArticles}&apiKey=${apiKey}`
    
    console.log('📡 Lade News...')
    
    const response = await fetch(url)
    
    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status}`)
    }
    
    const data = await response.json()
    
    // NewsAPI gibt Fehler manchmal als JSON zurück
    if (data.status === 'error') {
      throw new Error(data.message)
    }
    
    console.log('✅ News geladen:', data.articles.length)
    articles.value = data.articles
    
  } catch (e) {
    error.value = e instanceof Error ? e.message : 'Fehler beim Laden der News'
    console.error('❌ News API Fehler:', e)
  } finally {
    loading.value = false
  }
}

// Beim Start laden
onMounted(() => {
  fetchNews()
})

// Hilfsfunktion: Relative Zeit
const timeAgo = (dateString: string) => {
  const date = new Date(dateString)
  const now = new Date()
  const seconds = Math.floor((now.getTime() - date.getTime()) / 1000)
  
  if (seconds < 60) return 'Gerade eben'
  if (seconds < 3600) return `vor ${Math.floor(seconds / 60)} Min`
  if (seconds < 86400) return `vor ${Math.floor(seconds / 3600)} Std`
  return `vor ${Math.floor(seconds / 86400)} Tagen`
}

// Hilfsfunktion: Text kürzen
const truncate = (text: string, maxLength: number) => {
  if (!text) return 'Keine Beschreibung verfügbar'
  if (text.length <= maxLength) return text
  return text.substring(0, maxLength) + '...'
}
</script>

<template>
  <div class="news-widget">
    <div class="news-header">
      <h3 class="news-title">// TECH_NEWS</h3>
      <button 
        @click="fetchNews" 
        class="refresh-button"
        :disabled="loading"
      >
        🔄
      </button>
    </div>

    <!-- Loading State -->
    <div v-if="loading" class="loading-state">
      <div class="spinner"></div>
      <p>Lade Nachrichten...</p>
    </div>

    <!-- Error State -->
    <div v-else-if="error" class="error-state">
      <span class="error-icon">⚠️</span>
      <p>{{ error }}</p>
      <button @click="fetchNews" class="retry-button">
        Erneut versuchen
      </button>
    </div>

    <!-- Success State: News anzeigen -->
    <div v-else class="news-grid">
      
      <!-- v-for durch Artikel -->
      <article 
        v-for="(article, index) in articles" 
        :key="index"
        class="news-card"
      >
        <!-- News Image (falls vorhanden) -->
        <div class="news-image-container">
          <img 
            v-if="article.urlToImage" 
            :src="article.urlToImage" 
            :alt="article.title"
            class="news-image"
            @error="$event.target.style.display='none'"
          />
          <div v-else class="news-image-placeholder">
            📰
          </div>
        </div>

        <!-- News Content -->
        <div class="news-content">
          <!-- Source & Time -->
          <div class="news-meta">
            <span class="news-source">{{ article.source.name }}</span>
            <span class="news-time">{{ timeAgo(article.publishedAt) }}</span>
          </div>

          <!-- Title -->
          <h4 class="news-headline">
            {{ truncate(article.title, 80) }}
          </h4>

          <!-- Description -->
          <p class="news-description">
            {{ truncate(article.description || '', 100) }}
          </p>

          <!-- Link -->
          <a 
            :href="article.url" 
            target="_blank"
            rel="noopener noreferrer"
            class="news-link"
          >
            Artikel lesen →
          </a>
        </div>
      </article>

      <!-- Falls keine Artikel -->
      <div v-if="articles.length === 0" class="no-news">
        <p>Keine Nachrichten gefunden 📭</p>
      </div>

    </div>
  </div>
</template>

<style scoped>
.news-widget {
  margin-top: 2rem;
}

.news-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.news-title {
  color: #58a6ff;
  font-family: 'Courier New', monospace;
  font-size: 1.3rem;
  margin: 0;
  padding-left: 0.5rem;
  border-left: 3px solid #58a6ff;
}

.refresh-button {
  background: transparent;
  border: 2px solid #58a6ff;
  color: #58a6ff;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.2rem;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.refresh-button:hover:not(:disabled) {
  background: #58a6ff;
  transform: rotate(180deg);
}

.refresh-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
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
  padding: 2rem;
  color: #f0883e;
  background: rgba(240, 136, 62, 0.1);
  border: 1px solid #f0883e;
  border-radius: 8px;
}

.error-icon {
  font-size: 2rem;
  display: block;
  margin-bottom: 0.5rem;
}

.retry-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background: #f0883e;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: opacity 0.2s;
}

.retry-button:hover {
  opacity: 0.8;
}

/* News Grid */
.news-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}

/* News Card */
.news-card {
  background: var(--terminal-bg, #161b22);
  border: 1px solid #30363d;
  border-radius: 8px;
  overflow: hidden;
  transition: all 0.2s;
  display: flex;
  flex-direction: column;
}

.news-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
  border-color: #58a6ff;
}

/* News Image */
.news-image-container {
  width: 100%;
  height: 180px;
  overflow: hidden;
  background: #21262d;
  display: flex;
  align-items: center;
  justify-content: center;
}

.news-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.news-image-placeholder {
  font-size: 4rem;
  opacity: 0.5;
}

/* News Content */
.news-content {
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.news-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
  font-size: 0.8rem;
}

.news-source {
  color: #7ee787;
  font-weight: 600;
  text-transform: uppercase;
}

.news-time {
  color: #8b949e;
}

.news-headline {
  color: var(--text-color, #c9d1d9);
  font-size: 1rem;
  font-weight: 600;
  line-height: 1.4;
  margin: 0 0 0.75rem 0;
}

.news-description {
  color: #8b949e;
  font-size: 0.9rem;
  line-height: 1.5;
  margin: 0 0 1rem 0;
  flex-grow: 1;
}

.news-link {
  color: #58a6ff;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.9rem;
  transition: color 0.2s;
  align-self: flex-start;
  padding: 0.5rem 0;
}

.news-link:hover {
  color: #7ee787;
}

.no-news {
  grid-column: 1 / -1;
  text-align: center;
  padding: 3rem;
  color: #8b949e;
}
</style>