<template>
  <section id="backbone" class="backbone-section">
    <div class="backbone-container">
      <!-- Section Title -->
      <div class="section-header">
        <span class="section-label block-label">Centralized Infrastructure</span>
        <h2 class="section-title">The Operational Backbone</h2>
        <p class="section-desc">
          By consolidating operational complexity under one master backbone, we enable media and esports organizations to unlock massive synergy.
        </p>
      </div>

      <!-- Alternating Layers -->
      <div class="layers-list">
        <div 
          v-for="(layer, idx) in layers" 
          :key="idx" 
          class="layer-item"
          :class="{ 'layer-item--reverse': idx % 2 !== 0 }"
        >
          <!-- Text Content -->
          <div class="layer-content">
            <!-- Icon Block -->
            <div class="icon-block">
              <component :is="layer.icon" />
            </div>

            <h3 class="layer-title">{{ layer.title }}</h3>
            <h4 class="layer-subtitle" :style="{ color: layer.badgeColor }">
              {{ layer.subtitle }}
            </h4>
            <p class="layer-desc">
              {{ layer.description }}
            </p>

            <ul class="layer-bullets">
              <li v-for="(bullet, bidx) in layer.bullets" :key="bidx" class="bullet-item">
                <span class="bullet-check" :style="{ color: layer.badgeColor }">✓</span>
                <span>{{ bullet }}</span>
              </li>
            </ul>
          </div>

          <!-- Graphical Representation Card -->
          <div class="graphical-card">
            <!-- Background decoration -->
            <div class="card-glow" :style="{ background: `radial-gradient(circle, ${layer.badgeColor}15 0%, transparent 70%)` }" />

            <!-- Flow Visualization Box -->
            <div class="flow-box">
              <div class="flow-step step-source">
                <span class="step-emoji">🏢</span>
                <div>
                  <div class="step-label">Source</div>
                  <div class="step-value">Zettasyn Backbone</div>
                </div>
              </div>

              <div class="flow-line" :style="{ background: `linear-gradient(to bottom, ${layer.badgeColor}, transparent)` }" />

              <div class="flow-step step-pillar" :style="{ borderColor: layer.badgeColor + '50', boxShadow: `0 0 20px ${layer.badgeColor}08` }">
                <span class="step-emoji">⚡</span>
                <div>
                  <div class="step-label" :style="{ color: layer.badgeColor }">Pillar Layer</div>
                  <div class="step-value-bold">{{ layer.title }}</div>
                </div>
              </div>

              <div class="flow-line" :style="{ background: `linear-gradient(to bottom, transparent, ${layer.badgeColor})` }" />

              <div class="flow-step step-target">
                <span class="step-emoji">🚀</span>
                <div>
                  <div class="step-label">Target Impact</div>
                  <div class="step-value">Global Scale Ventures</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { h } from 'vue'

const layers = [
  {
    title: 'Operational Excellence',
    subtitle: 'Streamlining administration so creatives can create.',
    description:
      'We deliver a unified back-office service that handles corporate governance, talent representation, contract law, IP licensing, and localized production logistics. By consolidating these complex administrative burdens, our ventures scale without operational drag.',
    bullets: [
      'Intellectual property & rights management',
      'Unified HR, legal, and compliance pipelines',
      'Advanced production staging and studio scheduling',
    ],
    badgeColor: '#a855f7',
    icon: {
      render: () => h('svg', { width: '40', height: '40', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '1.5', strokeLinecap: 'round', strokeLinejoin: 'round', style: { color: '#a855f7' } }, [
        h('path', { d: 'M12 22c5.523 0 10-4.477 10-10S17.523 2 12 2 2 6.477 2 12s4.477 10 10 10z' }),
        h('path', { d: 'm9 12 2 2 4-4' })
      ])
    }
  },
  {
    title: 'Financial Backbone',
    subtitle: 'Smarter capital deployment and revenue flows.',
    description:
      'Our centralized financial engine facilitates multi-currency accounting, tax compliance across global esports/media markets, and automated revenue splitting. We provide the capital reserve access and modeling necessary for large-scale content acquisitions.',
    bullets: [
      'Automated royalty & revenue-share distributions',
      'Consolidated multi-currency cash flow modeling',
      'Strategic capital allocation & venture seed modeling',
    ],
    badgeColor: '#ec4899',
    icon: {
      render: () => h('svg', { width: '40', height: '40', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '1.5', strokeLinecap: 'round', strokeLinejoin: 'round', style: { color: '#ec4899' } }, [
        h('line', { x1: '12', x2: '12', y1: '2', y2: '22' }),
        h('path', { d: 'M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6' })
      ])
    }
  },
  {
    title: 'Technical Scalability',
    subtitle: 'Enterprise software engineered for digital reach.',
    description:
      'We design and deploy proprietary ERP dashboards, digital distribution pipelines, and telemetry platforms that capture real-time audience metrics. Our tech infrastructure handles spikes in traffic and secures critical media assets.',
    bullets: [
      'Proprietary back-office ERP & analytics software',
      'Low-latency content distribution network architectures',
      'Robust user management & secure payment gateways',
    ],
    badgeColor: '#3b82f6',
    icon: {
      render: () => h('svg', { width: '40', height: '40', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '1.5', strokeLinecap: 'round', strokeLinejoin: 'round', style: { color: '#3b82f6' } }, [
        h('path', { d: 'M4.5 16.5c-1.5 1.26-2.5 3.19-2.5 5.5h20c0-2.31-1-4.24-2.5-5.5' }),
        h('path', { d: 'M12 2a5 5 0 0 0-5 5v3a5 5 0 0 0 10 0V7a5 5 0 0 0-5-5z' }),
        h('path', { d: 'M6 16v-3a6 6 0 0 1 12 0v3' })
      ])
    }
  }
]
</script>

<style scoped>
.backbone-section {
  padding: 100px 24px;
  background: #09090b;
  border-top: 1px solid #18181b;
  position: relative;
  z-index: 5;
}

.backbone-container {
  max-width: 1100px;
  margin: 0 auto;
}

.section-header {
  text-align: center;
  margin-bottom: 80px;
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

.layers-list {
  display: flex;
  flex-direction: column;
  gap: 80px;
}

.layer-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 48px;
}

.layer-item--reverse {
  flex-direction: row-reverse;
}

.layer-content {
  flex: 1 1 450px;
  max-width: 520px;
}

.icon-block {
  width: 64px;
  height: 64px;
  border-radius: 16px;
  background: rgba(24, 24, 27, 0.6);
  border: 1px solid #27272a;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 28px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.3);
}

.layer-title {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 32px;
  color: #ffffff;
  margin-bottom: 6px;
}

.layer-subtitle {
  font-family: 'Outfit', sans-serif;
  font-weight: 500;
  font-size: 16px;
  margin-bottom: 20px;
}

.layer-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 15px;
  font-weight: 300;
  line-height: 1.7;
  color: #a1a1aa;
  margin-bottom: 24px;
}

.layer-bullets {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.bullet-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-family: 'Outfit', sans-serif;
  font-size: 14px;
  color: #d4d4d8;
}

.bullet-check {
  margin-top: 2px;
  font-weight: bold;
}

.graphical-card {
  flex: 1 1 350px;
  background: linear-gradient(135deg, rgba(24,24,27,0.4) 0%, rgba(9,9,11,0.6) 100%);
  border: 1px solid #27272a;
  border-radius: 24px;
  padding: 48px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.05), 0 20px 40px rgba(0,0,0,0.3);
  position: relative;
  overflow: hidden;
}

.card-glow {
  position: absolute;
  top: 0;
  right: 0;
  width: 120px;
  height: 120px;
  filter: blur(20px);
}

.flow-box {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 16px;
  z-index: 2;
}

.flow-step {
  padding: 16px 20px;
  background: rgba(9,9,11,0.8);
  border: 1px solid #27272a;
  border-radius: 12px;
  display: flex;
  align-items: center;
  gap: 14px;
}

.step-source {
  transform: translateX(-12px);
}

.step-target {
  transform: translateX(12px);
}

.step-emoji {
  font-size: 18px;
}

.step-label {
  font-size: 11px;
  color: #71717a;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.step-value {
  font-size: 14px;
  color: #ffffff;
  font-weight: 500;
}

.step-value-bold {
  font-size: 14px;
  color: #ffffff;
  font-weight: 600;
}

.flow-line {
  height: 32px;
  width: 2px;
  margin-left: 36px;
}
</style>
