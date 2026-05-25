<template>
  <section id="careers" class="careers-section">
    <div class="careers-container">
      <!-- Section Header -->
      <div class="section-header">
        <span class="section-label block-label">Careers & Culture</span>
        <h2 class="section-title">Shape the Future of Media & Tech</h2>
        <p class="section-desc">
          We are looking for elite operators, engineers, legal minds, and creatives who thrive under high autonomy and are ready to construct global scale.
        </p>
      </div>

      <!-- Culture Grid -->
      <div class="culture-grid">
        <div 
          v-for="(v, idx) in values" 
          :key="idx" 
          class="value-card"
          :class="{ 'card--hovered': hoveredIdx === idx }"
          @mouseenter="hoveredIdx = idx"
          @mouseleave="hoveredIdx = null"
        >
          <div class="value-emoji" :class="{ 'emoji--active': hoveredIdx === idx }">
            {{ v.emoji }}
          </div>
          <h3 class="value-title">{{ v.title }}</h3>
          <p class="value-desc">{{ v.desc }}</p>
        </div>
      </div>

      <!-- Action Callout Box -->
      <div class="callout-box">
        <!-- Subtle decoration lines -->
        <div class="decoration-line" />

        <div class="callout-content">
          <h3 class="callout-title">Ready to Join the Synergy?</h3>
          <p class="callout-desc">
            Explore our parent organization and venture team roles across development, legal affairs, financial operations, and content production.
          </p>
          <a href="mailto:careers@zettasyn.com" class="gradient-btn callout-btn">
            View Open Roles →
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const hoveredIdx = ref(null)

const values = [
  {
    title: 'Ecosystem Synergy',
    desc: 'We do not build in silos. Every solution, pipeline, and structural innovation we develop is engineered to benefit and lift all ventures across our network.',
    emoji: '🤝',
  },
  {
    title: 'Absolute Ownership',
    desc: 'We trust our teams with complete autonomy. From day one, you are expected to take full custody of your projects, decisions, and impact.',
    emoji: '👑',
  },
  {
    title: 'Uncompromised Quality',
    desc: 'We build enterprise-grade operations. We believe in high standards of design, speed, code hygiene, and scalability.',
    emoji: '🏆',
  },
]
</script>

<style scoped>
.careers-section {
  padding: 100px 24px;
  background: #09090b;
  position: relative;
  z-index: 5;
}

.careers-container {
  max-width: 1100px;
  margin: 0 auto;
}

.section-header {
  text-align: center;
  margin-bottom: 70px;
}

.block-label {
  display: block;
  margin-bottom: 12px;
}

.section-title {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: clamp(28px, 5vw, 42px);
  color: #ffffff;
  letter-spacing: -0.01em;
  margin-bottom: 16px;
}

.section-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 16px;
  font-weight: 300;
  color: #71717a;
  max-width: 600px;
  margin: 0 auto;
}

.culture-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  margin-bottom: 64px;
}

.value-card {
  background: rgba(24, 24, 27, 0.2);
  border: 1px solid #1f1f23;
  border-radius: 16px;
  padding: 36px;
  transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.value-card.card--hovered {
  border-color: #52525b;
  transform: translateY(-4px);
  box-shadow: 0 12px 30px rgba(0,0,0,0.3);
}

.value-emoji {
  font-size: 28px;
  margin-bottom: 20px;
  opacity: 0.7;
  transition: opacity 0.3s ease;
}

.value-emoji.emoji--active {
  opacity: 1;
}

.value-title {
  font-family: 'Outfit', sans-serif;
  font-weight: 600;
  font-size: 20px;
  color: #ffffff;
  margin-bottom: 12px;
}

.value-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 14px;
  font-weight: 300;
  line-height: 1.6;
  color: #a1a1aa;
}

.callout-box {
  background: linear-gradient(135deg, rgba(125,17,216,0.06) 0%, rgba(215,32,77,0.06) 100%);
  border: 1px solid #27272a;
  border-radius: 24px;
  padding: 50px 32px;
  text-align: center;
  backdrop-filter: blur(10px);
  position: relative;
  overflow: hidden;
}

.decoration-line {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 60%;
  height: 1px;
  background: linear-gradient(to right, transparent, rgba(125,17,216,0.3), transparent);
  transform: translate(-50%, -50%) rotate(-5deg);
  z-index: 1;
}

.callout-content {
  position: relative;
  z-index: 2;
}

.callout-title {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 26px;
  color: #ffffff;
  margin-bottom: 12px;
}

.callout-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 15px;
  font-weight: 300;
  color: #d4d4d8;
  max-width: 560px;
  margin: 0 auto 28px auto;
  line-height: 1.5;
}

.callout-btn {
  display: inline-flex;
  align-items: center;
  padding: 12px 24px;
  border-radius: 8px;
  font-size: 14px;
  text-decoration: none;
  box-shadow: 0 4px 15px rgba(215, 32, 77, 0.2);
}
</style>
