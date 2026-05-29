<template>
  <section id="about" class="hero-section">
    <!-- Dynamic Background Glows -->
    <div class="glow-orb glow-orb--top" />
    <div class="glow-orb glow-orb--bottom" />

    <!-- Grid Pattern overlay -->
    <div class="grid-overlay" />

    <div class="hero-container fade-up" :class="{ 'opacity-100': animated }">
      <!-- Company Subtitle Badge -->
      <div class="subtitle-badge">
        <span class="badge-dot" />
        <span class="badge-text">The Operational Backbone</span>
      </div>

      <!-- Hero Main Heading -->
      <h1 class="hero-heading">
        Centralized Infrastructure for <br class="br--desktop" />
        <span class="gradient-text">Creative Industries</span>
      </h1>

      <!-- Hero Paragraph Description -->
      <p class="hero-desc">
        Zettasyn is a parent holding company building the modern operational, financial, 
        and technological backbone across media, esports, studio production, and digital ecosystems.
      </p>

      <!-- Action Buttons -->
      <div class="hero-actions">
        <a href="#ecosystem" class="gradient-btn hero-btn hero-btn--primary">
          Explore Ecosystem
        </a>
        <a href="#contact" class="ghost-btn hero-btn hero-btn--secondary">
          Partner With Us
        </a>
      </div>

      <!-- Metric Counters Grid -->
      <div class="metrics-grid">
        <div v-for="(metric, idx) in metrics" :key="idx" class="metric-item">
          <div class="metric-value">{{ metric.value }}</div>
          <div class="metric-label">{{ metric.label }}</div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const animated = ref(false)

onMounted(() => {
  setTimeout(() => {
    animated.value = true
  }, 50)
})

const metrics = [
  { label: 'Ventures Powered', value: '6+' },
  { label: 'Audience Reach', value: '7M+' },
  { label: 'Capital Administered', value: '$14M+' },
  { label: 'Systems Uptime', value: '99.99%' },
]
</script>

<style scoped>
.hero-section {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 120px 24px 80px 24px;
  overflow: hidden;
  background: #09090b;
}

.glow-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  pointer-events: none;
  z-index: 1;
}

.glow-orb--top {
  top: 15%;
  left: 20%;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(125, 17, 216, 0.15) 0%, rgba(0, 0, 0, 0) 70%);
  animation: glowPulse 8s ease-in-out infinite;
}

.glow-orb--bottom {
  bottom: 15%;
  right: 20%;
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, rgba(215, 32, 77, 0.12) 0%, rgba(0, 0, 0, 0) 70%);
  animation: glowPulse 10s ease-in-out infinite alternate;
}

.grid-overlay {
  position: absolute;
  inset: 0;
  background-image: radial-gradient(#1f1f23 1px, transparent 1px);
  background-size: 24px 24px;
  opacity: 0.25;
  z-index: 2;
  pointer-events: none;
}

.hero-container {
  max-width: 1000px;
  text-align: center;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
  opacity: 0;
  transition: opacity 0.8s ease-out;
}

.hero-container.opacity-100 {
  opacity: 1;
}

.subtitle-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 16px;
  border-radius: 99px;
  background: rgba(24, 24, 27, 0.6);
  border: 1px solid #27272a;
  margin-bottom: 28px;
  backdrop-filter: blur(10px);
}

.badge-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--grad-to);
  box-shadow: 0 0 8px var(--grad-to);
}

.badge-text {
  font-family: 'Outfit', sans-serif;
  font-size: 13px;
  font-weight: 500;
  color: #d4d4d8;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

.hero-heading {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: clamp(40px, 8vw, 76px);
  line-height: 1.1;
  letter-spacing: -0.02em;
  color: #ffffff;
  margin-bottom: 24px;
}

.br--desktop {
  display: block;
}

.hero-desc {
  font-family: 'Outfit', sans-serif;
  font-weight: 300;
  font-size: clamp(16px, 2.5vw, 20px);
  line-height: 1.6;
  color: #a1a1aa;
  max-width: 720px;
  margin-bottom: 40px;
}

.hero-actions {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 16px;
  margin-bottom: 80px;
}

.hero-btn {
  display: inline-flex;
  align-items: center;
  padding: 14px 28px;
  border-radius: 8px;
  font-size: 15px;
  text-decoration: none;
}

.hero-btn--primary {
  box-shadow: 0 4px 20px rgba(125, 17, 216, 0.25);
}

.metrics-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 32px;
  width: 100%;
  max-width: 850px;
  border-top: 1px solid #1f1f23;
  padding-top: 48px;
}

.metric-item {
  text-align: center;
}

.metric-value {
  font-family: 'Outfit', sans-serif;
  font-size: clamp(28px, 4vw, 36px);
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 6px;
}

.metric-label {
  font-family: 'Outfit', sans-serif;
  font-size: 13px;
  font-weight: 400;
  color: #71717a;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

@keyframes glowPulse {
  0%, 100% {
    opacity: 0.25;
  }
  50% {
    opacity: 0.42;
  }
}

/* Mobile & Tablet Responsive Media Queries */
@media (max-width: 768px) {
  .hero-section {
    padding: 110px 20px 60px 20px;
    min-height: auto; /* Content flows naturally on mobile screen boundaries */
  }

  .br--desktop {
    display: none; /* Hide hard line breaks on smaller screens */
  }

  .hero-heading {
    font-size: clamp(30px, 7vw, 48px);
    line-height: 1.2;
    margin-bottom: 16px;
  }

  .hero-desc {
    font-size: 15px;
    margin-bottom: 32px;
    padding: 0 4px;
  }

  .hero-actions {
    width: 100%;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 60px;
    max-width: 300px; /* Elegant constrained buttons on mobile */
  }

  .hero-btn {
    width: 100%;
    justify-content: center;
    padding: 12px 24px;
    font-size: 14px;
  }

  .metrics-grid {
    grid-template-columns: repeat(2, 1fr); /* 2x2 grid layout is balanced and compact */
    gap: 24px;
    padding-top: 36px;
  }

  .metric-value {
    font-size: 26px;
  }

  .metric-label {
    font-size: 11px;
  }

  .glow-orb--top {
    width: 250px;
    height: 250px;
    top: 5%;
    left: 5%;
  }

  .glow-orb--bottom {
    width: 300px;
    height: 300px;
    bottom: 5%;
    right: 5%;
  }
}

@media (max-width: 380px) {
  .hero-section {
    padding: 100px 16px 48px 16px;
  }

  .hero-heading {
    font-size: 26px;
  }

  .metrics-grid {
    grid-template-columns: 1fr; /* Stack vertically on very narrow screens */
    gap: 20px;
    padding-top: 28px;
  }
}
</style>
