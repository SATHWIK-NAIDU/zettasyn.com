<template>
  <section id="firms" class="firms-section" ref="firmsSection">
    <div class="firms-content">
      <h2 class="section-title" ref="title">
        <span class="gradient-text">Our Ecosystem</span>
      </h2>
      <p class="section-desc" ref="desc">
        A constellation of specialized firms pushing the boundaries of technology and media.
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
            @mouseenter="onTileEnter($event, index)"
            @mouseleave="onTileLeave"
          >
            <div class="tile-glow" :class="`glow-${index}`"></div>
            <div class="firm-icon">
              <div class="icon-placeholder"></div>
            </div>
            <div class="firm-sector">{{ firm.sector }}</div>
            <h3 class="firm-name">{{ firm.name }}</h3>
            <p class="firm-desc">{{ firm.desc }}</p>
            <button class="firm-btn">
              <span>Explore</span>
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M5 12h14M12 5l7 7-7 7" />
              </svg>
            </button>
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
  { name: 'SynPlay', sector: 'Esports', desc: 'Next-generation competitive gaming leagues and athletic development.' },
  { name: 'ZettaStudios', sector: 'Production', desc: 'State-of-the-art virtual production and broadcast facilities.' },
  { name: 'AeroCine', sector: 'Cinematic Media', desc: 'Narrative-driven film and commercial cinematic storytelling.' },
  { name: 'PolyForge', sector: 'Game Development', desc: 'Immersive AAA gaming experiences and interactive media.' },
  { name: 'MotionSyn', sector: 'Motion Graphics', desc: 'High-end 3D animation, VFX, and motion design systems.' },
  { name: 'ZettaConsult', sector: 'IT Consultancy', desc: 'Enterprise digital transformation and scalable infrastructure.' },
  { name: 'NeuralCore', sector: 'AI Systems', desc: 'Advanced artificial intelligence and machine learning solutions.' },
  { name: 'LiveSyn', sector: 'Event Production', desc: 'Unforgettable live events and immersive physical experiences.' }
]

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
  gsap.to(tile, {
    scale: 1.05,
    z: 50,
    boxShadow: '0 20px 40px rgba(0,0,0,0.8), 0 0 30px rgba(0, 240, 255, 0.2)',
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
  cursor: pointer;
  overflow: hidden;
  transition: border-color 0.4s cubic-bezier(0.16, 1, 0.3, 1), box-shadow 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  transform-style: preserve-3d;
  background: rgba(17, 17, 21, 0.4);
  backdrop-filter: blur(12px);
  border: 1px solid var(--border-subtle);
  border-radius: 16px;
}

.firm-tile:hover {
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

.firm-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 8px;
}

.icon-placeholder {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: linear-gradient(135deg, var(--grad-from), var(--grad-to));
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

.firm-tile:hover .firm-btn::after {
  width: 100%;
}

.firm-btn svg {
  transition: transform 0.3s ease;
}

.firm-tile:hover .firm-btn svg {
  transform: translateX(4px);
}
</style>
