<script setup lang="ts">
import { ref } from 'vue'
import ProfileCard from './components/ProfileCard.vue'
import SkillBadge from './components/SkillBadge.vue'
import TerminalWindow from './components/TerminalWindow.vue'  // ← NEU
import GitHubRepos from './components/GitHubRepos.vue'  // ← NEU
import NewsWidget from './components/NewsWidget.vue'  // ← NEU
const isDarkMode = ref(true)

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value
}
</script>

<template>
  <div :class="{ dark: isDarkMode }" class="app-container">
    
    <!-- Header -->
    <header class="dashboard-header">
      <div class="header-content">
        <h1>// DEVELOPER.DASHBOARD</h1>
        <button @click="toggleTheme" class="theme-toggle">
          {{ isDarkMode ? '☀️' : '🌙' }}
        </button>
      </div>
    </header>

    <main class="dashboard-main">
      
      <!-- Cards Grid - Erste Reihe -->
      <div class="cards-grid">
        
        <!-- Profile Card -->
        <ProfileCard 
          name="DODOSACK"
          role="Full Stack Binge Watcher"
          status="Studying"
          status-color="#ff5f56"
        />

        <!-- Skills Section -->
        <div class="skills-section">
          <h3 class="section-title">// SKILLS</h3>
          <div class="skills-grid">
            <SkillBadge name="Vue.js" :level="4" category="Frontend" />
            <SkillBadge name="TypeScript" :level="3" category="Language" />
            <SkillBadge name="Git" :level="4" category="Tools" />
            <SkillBadge name="CSS" :level="3" category="Frontend" />
          </div>
        </div>
      </div>

      <!-- Terminal Section -->
      <div class="terminal-section">
        <h2 class="section-header">// SYSTEM_INFO</h2>
        
        <div class="terminal-grid">
          
          <!-- Terminal 1: About Me -->
          <TerminalWindow title="whoami.sh">
            <p class="terminal-line">
              <span class="prompt">$</span>
              <span class="command">cat about.txt</span>
            </p>
            <p class="terminal-output">
              Hey! Ich bin ein Developer in Ausbildung.<br>
              Aktuell lerne ich Vue.js und TypeScript!
            </p>
            <p class="terminal-line">
              <span class="prompt">$</span>
              <span class="command">echo $INTERESTS</span>
            </p>
            <p class="terminal-output success">
              Web Development | Binge Watching | Coffee ☕
            </p>
          </TerminalWindow>

          <!-- Terminal 2: Current Status -->
          <TerminalWindow title="status.sh">
            <p class="terminal-line">
              <span class="prompt">$</span>
              <span class="command">git status</span>
            </p>
            <p class="terminal-output">
              On branch main<br>
              Your branch is up to date.
            </p>
            <p class="terminal-line">
              <span class="prompt">$</span>
              <span class="command">npm run dev</span>
            </p>
            <p class="terminal-output success">
              ✓ Server running at http://localhost:5173
            </p>
            <p class="comment">// Learning Vue.js one component at a time...</p>
          </TerminalWindow>

          <!-- Terminal 3: Commands -->
          <TerminalWindow title="commands.sh">
            <p class="terminal-line">
              <span class="prompt">$</span>
              <span class="command">help</span>
            </p>
            <p class="terminal-output">
              Available commands:
            </p>
            <p class="terminal-output">
              → portfolio    (View my projects)<br>
              → skills       (List my abilities)<br>
              → contact      (Get in touch)<br>
              → coffee       (Brew some motivation)
            </p>
            <p class="terminal-line">
              <span class="prompt">$</span>
              <span class="command">coffee</span>
            </p>
            <p class="terminal-output success">☕ Brewing...</p>
          </TerminalWindow>
          
        </div>
      </div>
      
    </main>
  </div>
  <!-- GitHub Repos Section -->
<GitHubRepos username="Dodosack" :max-repos="6" />

<!-- News Section -->
<NewsWidget category="technology" :max-articles="4" />


</template>

<style scoped>
/* Root Variablen */
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

.app-container.dark {
  --bg-color: #0d1117;
  --text-color: #c9d1d9;
  --terminal-bg: #161b22;
  --terminal-text: #58a6ff;
}

/* Header */
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

/* Section Headers */
.section-header {
  color: var(--terminal-text);
  font-family: 'Courier New', monospace;
  font-size: 1.3rem;
  margin: 2rem 0 1rem 0;
  padding-left: 0.5rem;
  border-left: 3px solid var(--terminal-text);
}

/* Cards Grid */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.skills-section {
  background: var(--terminal-bg);
  border: 1px solid #30363d;
  border-radius: 8px;
  padding: 1.5rem;
}

.section-title {
  color: #58a6ff;
  font-family: 'Courier New', monospace;
  font-size: 1.2rem;
  margin: 0 0 1rem 0;
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

/* Terminal Section */
.terminal-section {
  margin-top: 2rem;
}

.terminal-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 1.5rem;
}
</style>