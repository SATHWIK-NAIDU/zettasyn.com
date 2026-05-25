<template>
  <section id="ecosystem" class="ecosystem-section">
    <div class="ecosystem-container">
      <!-- Section Header -->
      <div class="section-header">
        <span class="section-label block-label">Ecosystem Verticals</span>
        <h2 class="section-title">Ventures & Operational Pillars</h2>
        <p class="section-desc">
          We manage, scale, and integrate essential operational layers for modern creators and builders across distinct digital verticals.
        </p>
      </div>

      <!-- Card Grid -->
      <div class="cards-grid">
        <div 
          v-for="(v, idx) in verticals" 
          :key="idx" 
          class="ecosystem-card"
          :class="{ 'card--hovered': hoveredIdx === idx }"
          @mouseenter="hoveredIdx = idx"
          @mouseleave="hoveredIdx = null"
        >
          <!-- Card Top Row -->
          <div class="card-top">
            <div class="icon-wrapper" :class="{ 'icon-wrapper--active': hoveredIdx === idx }">
              <component :is="v.icon" />
            </div>
            <span class="tag-badge" :class="{ 'tag-badge--active': hoveredIdx === idx }">
              {{ v.tag }}
            </span>
          </div>

          <!-- Card Body -->
          <h3 class="card-title">{{ v.title }}</h3>
          <p class="card-desc">{{ v.description }}</p>

          <!-- Metric Footer -->
          <div class="card-footer">
            <span class="footer-label">Key Indicator</span>
            <span class="footer-metric" :class="{ 'footer-metric--active': hoveredIdx === idx }">
              {{ v.metric }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, h } from 'vue'

const hoveredIdx = ref(null)

const verticals = [
  {
    title: 'Media & Entertainment',
    description: 'Nurturing premier digital talent, entertainment brands, and digital-first broadcasting networks at global scale.',
    metric: '18M+ Audience',
    tag: 'Distribution',
    icon: {
      render: () => h('svg', { width: '24', height: '24', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '2', strokeLinecap: 'round', strokeLinejoin: 'round' }, [
        h('rect', { width: '20', height: '15', x: '2', y: '3', rx: '2' }),
        h('path', { d: 'M12 18v4M8 22h8M2 8h20' })
      ])
    }
  },
  {
    title: 'Esports & Gaming',
    description: 'Providing scalable operating systems, player management, tournament infrastructure, and community hubs.',
    metric: '100+ Events',
    tag: 'Competition',
    icon: {
      render: () => h('svg', { width: '24', height: '24', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '2', strokeLinecap: 'round', strokeLinejoin: 'round' }, [
        h('path', { d: 'M6 12h4m-2-2v4m10-2h.01M15 12h.01M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z' })
      ])
    }
  },
  {
    title: 'Production & Studios',
    description: 'Investing in high-end physical studios, virtual staging equipment, post-production pipelines, and visual effects.',
    metric: '5+ Studios',
    tag: 'Creation',
    icon: {
      render: () => h('svg', { width: '24', height: '24', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '2', strokeLinecap: 'round', strokeLinejoin: 'round' }, [
        h('path', { d: 'm22 8-6 4 6 4V8Z' }),
        h('rect', { width: '14', height: '12', x: '2', y: '6', rx: '2', ry: '2' })
      ])
    }
  },
  {
    title: 'Digital Infrastructure',
    description: 'Building proprietary financial ledgers, cloud hosting, automated ERP panels, and scalable web solutions.',
    metric: '99.9% Uptime',
    tag: 'Technology',
    icon: {
      render: () => h('svg', { width: '24', height: '24', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '2', strokeLinecap: 'round', strokeLinejoin: 'round' }, [
        h('rect', { width: '20', height: '8', x: '2', y: '2', rx: '2' }),
        h('rect', { width: '20', height: '8', x: '2', y: '14', rx: '2' }),
        h('path', { d: 'M6 6h.01M6 18h.01M20 6h.01M20 18h.01' })
      ])
    }
  }
]
</script>

<style scoped>
.ecosystem-section {
  padding: 100px 24px;
  background: #09090b;
  position: relative;
  z-index: 5;
}

.ecosystem-container {
  max-width: 1200px;
  margin: 0 auto;
}

.section-header {
  text-align: center;
  margin-bottom: 60px;
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

.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
}

.ecosystem-card {
  background: rgba(24, 24, 27, 0.3);
  border: 1px solid #27272a;
  border-radius: 16px;
  padding: 40px 32px;
  transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  box-shadow: 0 4px 20px rgba(0,0,0,0.2);
  backdrop-filter: blur(10px);
  cursor: default;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.ecosystem-card.card--hovered {
  background: rgba(39, 39, 42, 0.4);
  border: 1px solid #52525b;
  transform: translateY(-6px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4), 0 0 20px rgba(125, 17, 216, 0.08);
}

.card-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 32px;
}

.icon-wrapper {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(39, 39, 42, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #a1a1aa;
  transition: all 0.3s ease;
}

.icon-wrapper.icon-wrapper--active {
  background: linear-gradient(135deg, var(--grad-from) 0%, var(--grad-to) 100%);
  color: #ffffff;
  box-shadow: 0 8px 16px rgba(125, 17, 216, 0.3);
}

.tag-badge {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: #71717a;
  background: rgba(39, 39, 42, 0.3);
  padding: 4px 10px;
  border-radius: 99px;
  border: 1px solid #27272a;
  transition: all 0.3s ease;
}

.tag-badge.tag-badge--active {
  color: var(--grad-glow);
}

.card-title {
  font-family: 'Outfit', sans-serif;
  font-weight: 600;
  font-size: 22px;
  color: #ffffff;
  margin-bottom: 12px;
}

.card-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 14px;
  font-weight: 300;
  line-height: 1.6;
  color: #a1a1aa;
  margin-bottom: 32px;
  flex-grow: 1;
}

.card-footer {
  border-top: 1px solid #27272a;
  padding-top: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.footer-label {
  font-size: 13px;
  color: #71717a;
  font-weight: 400;
}

.footer-metric {
  font-size: 14px;
  font-weight: 600;
  color: #d4d4d8;
  transition: color 0.3s ease;
}

.footer-metric.footer-metric--active {
  color: #ffffff;
}
</style>
