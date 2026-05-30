<template>
  <section id="firms" class="firms-section" ref="firmsSection">
    <div class="firms-content">
      <h2 class="section-title" ref="title">
        <span class="gradient-text">Our Ecosystem</span>
      </h2>
      <p class="section-desc" ref="desc">
        A constellation of specialized firms pushing the boundaries of technology, media, and esports.
      </p>

      <div class="firms-galaxy" ref="galaxy">
        <!-- Connecting Lines SVG -->
        <svg class="connections" preserveAspectRatio="none">
          <path d="M 300 200 Q 500 100, 700 300 T 1100 250" class="energy-line" />
          <path d="M 200 400 Q 600 500, 800 200 T 1000 450" class="energy-line line-alt" />
          <path d="M 100 250 Q 400 350, 900 150" class="energy-line" />
        </svg>

        <div class="firms-grid">
          <div 
            v-for="(firm, index) in firms" 
            :key="index"
            class="firm-tile glass-panel"
            :class="{ 'clickable': firm.website, 'is-empty-slot': !firm.name }"
            @mouseenter="onTileEnter($event, index)"
            @mouseleave="onTileLeave"
            @click="firm.website && openWebsite(firm.website)"
          >
            <div class="tile-glow" :class="`glow-${index}`"></div>
            
            <!-- Logo Section -->
            <div class="firm-logo-wrapper">
              <img v-if="firm.logo" :src="firm.logo" :alt="firm.name ? firm.name + ' Logo' : 'Venture Logo'" class="firm-logo-img" draggable="false" />
              <div v-else-if="firm.iconSvg" class="firm-logo-svg" v-html="firm.iconSvg"></div>
              <div v-else class="firm-logo-placeholder">
                <span>{{ firm.name ? firm.name.charAt(0) : '+' }}</span>
              </div>
            </div>

            <div class="firm-sector">{{ firm.sector }}</div>
            <h3 class="firm-name">{{ firm.name || 'Incubating Venture' }}</h3>
            <p class="firm-desc">{{ firm.desc }}</p>
            
            <!-- Redirect Link Button -->
            <a 
              v-if="firm.website" 
              :href="firm.website" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="firm-btn"
              @click.stop
            >
              <span>Visit Website</span>
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6" />
                <polyline points="15 3 21 3 21 9" />
                <line x1="10" y1="14" x2="21" y2="3" />
              </svg>
            </a>
            <span v-else class="firm-btn disabled">
              <span>Under Incubation</span>
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <circle cx="12" cy="12" r="10" />
                <polyline points="12 6 12 12 16 14" />
              </svg>
            </span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

if (process.client) {
  gsap.registerPlugin(ScrollTrigger)
}

const firmsSection = ref(null)
const title = ref(null)
const desc = ref(null)
const galaxy = ref(null)

const firms = [
  {
    name: 'Hyperlax',
    sector: 'Broadcasting Services',
    desc: 'Cutting-edge live broadcasting, stream transmission infrastructure, and high-fidelity real-time media services.',
    website: 'https://hyperlax.com',
    logo: '',
    iconSvg: `<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: var(--grad-to)">
      <path d="M4.9 19.1C1 15.2 1 8.8 4.9 4.9M7.7 16.3c-2.3-2.3-2.3-6.2 0-8.5M12 12h.01M16.3 7.7c2.3 2.3 2.3 6.2 0 8.5M19.1 4.9C23 8.8 23 15.2 19.1 19.1" />
    </svg>`
  },
  {
    name: 'Webturtle',
    sector: 'IT Services',
    desc: 'Enterprise-grade cloud consulting, custom infrastructure engineering, network architecture, and scalable software pipelines.',
    website: 'https://webturtle.com',
    logo: '',
    iconSvg: `<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: var(--grad-to)">
      <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z" />
      <path d="M12 8v8M8 12h8" />
    </svg>`
  },
  {
    name: 'The Motion Theory',
    sector: 'Creative & Graphics',
    desc: 'Avant-garde 3D motion design, immersive visual effects (VFX), premium corporate design, and CGI graphic production.',
    website: 'https://themotiontheory.com',
    logo: '',
    iconSvg: `<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: var(--grad-to)">
      <path d="M4.5 16.5c-1.5-1.5-2.5-3.5-2.5-5.5C2 6.5 6.5 2 12 2c4 0 7.5 2.5 9 6M19.5 7.5c1.5 1.5 2.5 3.5 2.5 5.5c0 4.5-4.5 9-10 9c-4 0-7.5-2.5-9-6" />
      <circle cx="12" cy="12" r="3" />
    </svg>`
  },
  {
    name: 'Offbit Studios',
    sector: 'Game Development & Testing',
    desc: 'End-to-end interactive entertainment creation, immersive gameplay engineering, and comprehensive QA testing architectures.',
    website: 'https://offbitstudios.com',
    logo: '',
    iconSvg: `<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: var(--grad-to)">
      <rect x="2" y="6" width="20" height="12" rx="3" />
      <path d="M6 12h4M8 10v4M16 11h.01M18 13h.01" />
    </svg>`
  },
  {
    name: 'National Esports',
    sector: 'Esports & Event Management',
    desc: 'Premier esports league management, tournament hosting, concerts, high-profile corporate conventions, and live experiential event design.',
    website: 'https://nationalesports.in',
    logo: '',
    iconSvg: `<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: var(--grad-to)">
      <path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6M18 9h1.5a2.5 2.5 0 0 0 0-5H18M4 22h16M10 14.66V17c0 .55-.45 1-1 1H4v2h16v-2h-5c-.55 0-1-.45-1-1v-2.34" />
      <path d="M12 2a5 5 0 0 1 5 5v5a5 5 0 0 1-10 0V7a5 5 0 0 1 5-5z" />
    </svg>`
  },
  {
    name: '',
    sector: 'Pending Incubation',
    desc: 'This slot is reserved for our next strategic market horizontal. Currently evaluating technical and visual architecture pipelines.',
    website: '',
    logo: '',
    iconSvg: `<svg viewBox="0 0 24 24" width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: rgba(255, 255, 255, 0.2)">
      <line x1="12" y1="5" x2="12" y2="19" />
      <line x1="5" y1="12" x2="19" y2="12" />
    </svg>`
  }
]

const openWebsite = (url) => {
  if (url && typeof window !== 'undefined') {
    window.open(url, '_blank')
  }
}

onMounted(() => {
  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: firmsSection.value,
      start: 'top 75%',
      end: 'bottom 20%',
      toggleActions: 'play none none reverse'
    }
  })

  tl.fromTo(title.value,
    { opacity: 0, x: -50 },
    { opacity: 1, x: 0, duration: 0.8, ease: 'power3.out' }
  )

  tl.fromTo(desc.value,
    { opacity: 0, x: -50 },
    { opacity: 1, x: 0, duration: 0.8, ease: 'power3.out' },
    "-=0.6"
  )

  // Outer frame reveal
  const tiles = galaxy.value.querySelectorAll('.firm-tile')
  tl.fromTo(tiles,
    { opacity: 0, scale: 0.95, y: 80, rotationX: 15 },
    { opacity: 1, scale: 1, y: 0, rotationX: 0, duration: 0.8, stagger: 0.15, ease: 'power3.out', transformOrigin: 'top center' },
    "-=0.4"
  )

  // Inner details reveal
  const firmContent = galaxy.value.querySelectorAll('.firm-tile > *:not(.tile-glow)')
  tl.fromTo(firmContent,
    { opacity: 0, y: 10, filter: 'blur(4px)' },
    { opacity: 1, y: 0, filter: 'blur(0px)', duration: 0.5, stagger: 0.05, ease: 'power2.out' },
    "-=1.0"
  )

  const lines = galaxy.value.querySelectorAll('.energy-line')
  tl.fromTo(lines,
    { strokeDasharray: 2000, strokeDashoffset: 2000 },
    { strokeDashoffset: 0, duration: 2, stagger: 0.2, ease: 'power2.inOut' },
    "-=1.2"
  )
})

function onTileEnter(e, index) {
  const tile = e.currentTarget
  // Only apply aggressive pop/glow if it is a clickable active firm
  const isClickable = !tile.classList.contains('is-empty-slot')
  
  gsap.to(tile, {
    scale: 1.05,
    z: 50,
    boxShadow: isClickable 
      ? '0 20px 40px rgba(0,0,0,0.8), 0 0 30px rgba(0, 240, 255, 0.2)'
      : '0 15px 30px rgba(0,0,0,0.7), 0 0 15px rgba(255, 255, 255, 0.05)',
    duration: 0.4,
    ease: 'power3.out'
  })
  
  // Illuminate specific lines (mock interaction)
  gsap.to('.energy-line', {
    stroke: 'rgba(255, 255, 255, 0.5)',
    duration: 0.3
  })
}

function onTileLeave(e) {
  const tile = e.currentTarget
  gsap.to(tile, {
    scale: 1,
    z: 0,
    boxShadow: 'none',
    duration: 0.4,
    ease: 'power3.out'
  })
  
  gsap.to('.energy-line', {
    stroke: 'rgba(0, 240, 255, 0.2)',
    duration: 0.3
  })
}
</script>

<style scoped>
.firms-section {
  position: relative;
  padding: 160px 24px;
  background: transparent;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
}

.firms-content {
  max-width: 1400px;
  width: 100%;
  position: relative;
  z-index: 2;
}

.section-title {
  font-family: var(--font-heading);
  font-size: clamp(40px, 6vw, 64px);
  font-weight: 700;
  margin-bottom: 16px;
  letter-spacing: -0.02em;
}

.section-desc {
  font-family: var(--font-body);
  font-size: clamp(16px, 2vw, 20px);
  color: var(--text-secondary);
  max-width: 600px;
  margin-bottom: 80px;
}

.firms-galaxy {
  position: relative;
  width: 100%;
  perspective: 1200px;
}

.connections {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}

.energy-line {
  fill: none;
  stroke: rgba(0, 240, 255, 0.2);
  stroke-width: 2;
  filter: drop-shadow(0 0 8px rgba(0, 240, 255, 0.5));
  transition: stroke 0.3s;
}

.line-alt {
  stroke: rgba(157, 0, 255, 0.2);
  filter: drop-shadow(0 0 8px rgba(157, 0, 255, 0.5));
}

.firms-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  position: relative;
  z-index: 2;
}

.firm-tile {
  position: relative;
  padding: 32px 24px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  overflow: hidden;
  transition: border-color 0.4s cubic-bezier(0.16, 1, 0.3, 1), box-shadow 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  transform-style: preserve-3d;
  background: rgba(17, 17, 21, 0.4);
  backdrop-filter: blur(12px);
  border: 1px solid var(--border-subtle);
  border-radius: 16px;
}

/* Clickable card style */
.firm-tile.clickable {
  cursor: pointer;
}

.firm-tile.clickable:hover {
  border-color: rgba(0, 240, 255, 0.4);
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.6), inset 0 0 20px rgba(0, 240, 255, 0.05);
}

.tile-glow {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at center, rgba(0, 240, 255, 0.1) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.5s;
  pointer-events: none;
}

.firm-tile:hover .tile-glow {
  opacity: 1;
}

/* Logo container & slot styling */
.firm-logo-wrapper {
  width: 56px;
  height: 56px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 8px;
  overflow: hidden;
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  position: relative;
  box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.2);
}

.firm-tile.clickable:hover .firm-logo-wrapper {
  background: rgba(0, 240, 255, 0.05);
  border-color: rgba(0, 240, 255, 0.3);
  box-shadow: 0 0 15px rgba(0, 240, 255, 0.15), inset 0 0 8px rgba(0, 240, 255, 0.15);
  transform: translateY(-2px) translateZ(10px);
}

.firm-logo-img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  padding: 8px;
  transition: transform 0.4s ease;
}

.firm-tile:hover .firm-logo-img {
  transform: scale(1.08);
}

.firm-logo-svg {
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.4s ease, filter 0.4s ease;
}

.firm-tile.clickable:hover .firm-logo-svg {
  transform: scale(1.1) rotate(4deg);
  filter: drop-shadow(0 0 6px rgba(0, 240, 255, 0.5));
}

.firm-logo-placeholder {
  font-family: var(--font-heading);
  font-size: 20px;
  font-weight: 700;
  color: rgba(255, 255, 255, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Empty slot styling */
.firm-tile.is-empty-slot {
  border-style: dashed;
  border-color: rgba(255, 255, 255, 0.15);
  background: rgba(17, 17, 21, 0.15);
}

.firm-tile.is-empty-slot:hover {
  border-color: rgba(255, 255, 255, 0.3);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
}

.firm-tile.is-empty-slot .firm-name {
  color: rgba(255, 255, 255, 0.4);
  font-style: italic;
}

.firm-tile.is-empty-slot .firm-logo-wrapper {
  border-style: dashed;
  background: transparent;
  border-color: rgba(255, 255, 255, 0.12);
}

.firm-tile.is-empty-slot:hover .firm-logo-wrapper {
  border-color: rgba(255, 255, 255, 0.25);
  background: rgba(255, 255, 255, 0.02);
}

.firm-tile.is-empty-slot .firm-logo-svg svg {
  animation: pulseSlot 3s infinite ease-in-out;
}

@keyframes pulseSlot {
  0%, 100% {
    opacity: 0.3;
    transform: scale(1);
  }
  50% {
    opacity: 0.7;
    transform: scale(1.1);
  }
}

.firm-sector {
  font-family: var(--font-body);
  font-size: 11px;
  color: var(--grad-to);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  font-weight: 500;
}

.firm-name {
  font-family: var(--font-heading);
  font-size: 24px;
  font-weight: 600;
  color: #fff;
}

.firm-desc {
  font-family: var(--font-body);
  font-size: 14px;
  line-height: 1.6;
  color: var(--text-secondary);
  flex-grow: 1;
}

.firm-btn {
  margin-top: auto;
  align-self: flex-start;
  background: transparent;
  border: none;
  color: #fff;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  padding: 8px 0;
  position: relative;
  text-decoration: none;
}

.firm-btn::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 1px;
  background: var(--grad-to);
  transition: width 0.3s ease;
}

.firm-tile.clickable:hover .firm-btn::after {
  width: 100%;
}

.firm-btn svg {
  transition: transform 0.3s ease;
}

.firm-tile.clickable:hover .firm-btn svg {
  transform: translateX(4px);
}

.firm-btn.disabled {
  color: rgba(255, 255, 255, 0.25);
  cursor: default;
  pointer-events: none;
}

.firm-btn.disabled::after {
  display: none;
}

.firm-btn.disabled svg {
  animation: spinner 6s infinite linear;
}

@keyframes spinner {
  to {
    transform: rotate(360deg);
  }
}
</style>

