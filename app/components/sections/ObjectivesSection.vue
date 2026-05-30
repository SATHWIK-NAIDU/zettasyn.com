<template>
  <section id="objectives" class="objectives-section" ref="objectivesSection">
    <div class="objectives-content">
      <div class="title-wrapper">
        <h2 class="section-title" ref="title">
          Our <span class="gradient-text">Objectives</span>
        </h2>
        <div class="title-line" ref="titleLine"></div>
      </div>

      <div class="mission-grid" ref="grid">
        <div 
          v-for="(obj, index) in objectives" 
          :key="index"
          class="mission-card"
          @mousemove="onCardMove($event, index)"
          @mouseleave="onCardLeave(index)"
        >
          <div class="card-glass">
            <!-- Geometric background pattern -->
            <div class="geo-bg">
              <svg viewBox="0 0 100 100" preserveAspectRatio="none">
                <polygon :points="obj.polygon" class="geo-shape" />
              </svg>
            </div>
            
            <div class="card-inner-content">
              <div class="mission-header">
                <span class="mission-number">0{{ index + 1 }}</span>
                <div class="mission-icon-wrapper">
                  <div class="rotating-ring"></div>
                  <div class="inner-dot"></div>
                </div>
              </div>
              <h3 class="mission-title">{{ obj.title }}</h3>
              
              <div class="micro-ui">
                <div class="ui-bar"><div class="ui-progress"></div></div>
                <span class="ui-status">ACTIVE</span>
              </div>
            </div>
            
            <!-- Hover depth layer -->
            <div class="hover-depth" :ref="el => depthLayers[index] = el"></div>
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

const objectivesSection = ref(null)
const title = ref(null)
const titleLine = ref(null)
const grid = ref(null)
const depthLayers = ref([])

const objectives = [
  { title: 'Integrating an interconnected operational & technical backbone', polygon: '0,0 100,0 100,80 50,100 0,80' },
  { title: 'Pioneering ultra-low latency broadcasting & live stream networks', polygon: '20,0 80,0 100,50 80,100 20,100 0,50' },
  { title: 'Engineering secure, enterprise-grade cloud & IT infrastructures', polygon: '50,0 100,25 100,75 50,100 0,75 0,25' },
  { title: 'Pushing visual frontiers via avant-garde 3D motion & CGI graphics', polygon: '0,20 100,0 100,100 0,80' },
  { title: 'Crafting immersive interactive games with rigorous testing cycles', polygon: '10,10 90,0 100,90 0,100' },
  { title: 'Staging physical esports arenas, concerts, & experiential corporate events', polygon: '0,0 100,20 80,100 20,100' }
]

onMounted(() => {
  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: objectivesSection.value,
      start: 'top 70%',
      end: 'bottom 20%',
      toggleActions: 'play none none reverse'
    }
  })

  tl.fromTo(title.value,
    { opacity: 0, y: -30 },
    { opacity: 1, y: 0, duration: 0.8, ease: 'power3.out' }
  )

  tl.fromTo(titleLine.value,
    { scaleX: 0 },
    { scaleX: 1, duration: 1, ease: 'power4.out', transformOrigin: 'left' },
    "-=0.4"
  )

  const cards = grid.value.querySelectorAll('.mission-card')
  
  // Outer card framework reveal
  tl.fromTo(cards,
    { opacity: 0, y: 100, rotationX: 10, scale: 0.95 },
    { opacity: 1, y: 0, rotationX: 0, scale: 1, duration: 0.8, stagger: 0.15, ease: 'power3.out', transformOrigin: 'top center' },
    "-=0.6"
  )

  // Mechanical inner assembly: Geo shapes draw in
  const geoShapes = grid.value.querySelectorAll('.geo-shape')
  tl.fromTo(geoShapes,
    { strokeDasharray: 400, strokeDashoffset: 400 },
    { strokeDashoffset: 0, duration: 1.5, stagger: 0.1, ease: 'power2.inOut' },
    "-=0.5"
  )

  // Inner content boot sequence
  const innerContents = grid.value.querySelectorAll('.card-inner-content > *')
  tl.fromTo(innerContents,
    { opacity: 0, x: -20, filter: 'blur(5px)' },
    { opacity: 1, x: 0, filter: 'blur(0px)', duration: 0.6, stagger: 0.05, ease: 'power2.out' },
    "-=1.2"
  )
})

function onCardMove(e, index) {
  const card = e.currentTarget
  const rect = card.getBoundingClientRect()
  const x = (e.clientX - rect.left - rect.width / 2) / (rect.width / 2)
  const y = (e.clientY - rect.top - rect.height / 2) / (rect.height / 2)
  
  gsap.to(card, {
    rotationY: x * 8,
    rotationX: -y * 8,
    transformPerspective: 1000,
    duration: 0.4,
    ease: 'power2.out'
  })
  
  const depth = depthLayers.value[index]
  if (depth) {
    gsap.to(depth, {
      x: x * -15,
      y: y * -15,
      opacity: 1,
      duration: 0.3
    })
  }
}

function onCardLeave(index) {
  const cards = grid.value.querySelectorAll('.mission-card')
  const card = cards[index]
  
  gsap.to(card, {
    rotationY: 0,
    rotationX: 0,
    duration: 0.8,
    ease: 'elastic.out(1, 0.3)'
  })
  
  const depth = depthLayers.value[index]
  if (depth) {
    gsap.to(depth, {
      x: 0,
      y: 0,
      opacity: 0,
      duration: 0.5
    })
  }
}
</script>

<style scoped>
.objectives-section {
  position: relative;
  padding: 160px 24px;
  background: transparent;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  overflow: hidden;
}

.objectives-content {
  max-width: 1400px;
  width: 100%;
  position: relative;
  z-index: 2;
}

.title-wrapper {
  margin-bottom: 80px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.section-title {
  font-family: var(--font-heading);
  font-size: clamp(40px, 6vw, 64px);
  font-weight: 700;
  margin-bottom: 16px;
  letter-spacing: -0.02em;
}

.title-line {
  height: 2px;
  width: 120px;
  background: linear-gradient(to right, var(--grad-from), transparent);
}

.mission-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
  gap: 32px;
  perspective: 1200px;
}

.mission-card {
  position: relative;
  aspect-ratio: 4/3;
  cursor: pointer;
  transform-style: preserve-3d;
}

.card-glass {
  position: absolute;
  inset: 0;
  background: rgba(17, 17, 21, 0.4);
  backdrop-filter: blur(12px);
  border: 1px solid var(--border-subtle);
  border-radius: 20px;
  overflow: hidden;
  padding: 32px;
  display: flex;
  flex-direction: column;
  transform-style: preserve-3d;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.mission-card:hover .card-glass {
  border-color: rgba(0, 240, 255, 0.3);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5), inset 0 0 20px rgba(0, 240, 255, 0.05);
}

.geo-bg {
  position: absolute;
  inset: -20%;
  opacity: 0.05;
  transition: opacity 0.5s, transform 0.5s;
  pointer-events: none;
  z-index: 0;
}

.geo-shape {
  fill: none;
  stroke: var(--text-primary);
  stroke-width: 0.5;
  vector-effect: non-scaling-stroke;
}

.mission-card:hover .geo-bg {
  opacity: 0.15;
  transform: scale(1.1) rotate(5deg);
}

.mission-card:hover .geo-shape {
  stroke: var(--grad-to);
}

.card-inner-content {
  position: relative;
  z-index: 2;
  flex: 1;
  display: flex;
  flex-direction: column;
  transform: translateZ(20px);
}

.mission-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: auto;
}

.mission-number {
  font-family: var(--font-heading);
  font-size: 14px;
  font-weight: 600;
  color: var(--text-muted);
  letter-spacing: 0.1em;
}

.mission-icon-wrapper {
  position: relative;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.rotating-ring {
  position: absolute;
  inset: 0;
  border: 1px dashed var(--text-muted);
  border-radius: 50%;
  transition: border-color 0.3s;
}

.mission-card:hover .rotating-ring {
  animation: spin 4s linear infinite;
  border-color: var(--grad-to);
}

.inner-dot {
  width: 8px;
  height: 8px;
  background: var(--text-secondary);
  border-radius: 50%;
  transition: background 0.3s, box-shadow 0.3s;
}

.mission-card:hover .inner-dot {
  background: var(--grad-to);
  box-shadow: 0 0 10px var(--grad-to);
}

.mission-title {
  font-family: var(--font-heading);
  font-size: clamp(20px, 2vw, 26px);
  font-weight: 500;
  color: #fff;
  line-height: 1.2;
  margin-bottom: 24px;
}

.micro-ui {
  display: flex;
  align-items: center;
  gap: 12px;
}

.ui-bar {
  flex: 1;
  height: 2px;
  background: rgba(255, 255, 255, 0.1);
  position: relative;
  overflow: hidden;
}

.ui-progress {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 0%;
  background: var(--grad-to);
  transition: width 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.mission-card:hover .ui-progress {
  width: 100%;
}

.ui-status {
  font-family: var(--font-body);
  font-size: 10px;
  color: var(--text-muted);
  letter-spacing: 0.15em;
  transition: color 0.3s;
}

.mission-card:hover .ui-status {
  color: var(--grad-to);
}

.hover-depth {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at center, rgba(157, 0, 255, 0.1) 0%, transparent 60%);
  opacity: 0;
  pointer-events: none;
  z-index: 1;
  transform: translateZ(40px);
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

@media (max-width: 768px) {
  .objectives-section {
    padding: 80px 16px;
  }
  .title-wrapper {
    margin-bottom: 40px;
  }
  .mission-grid {
    grid-template-columns: 1fr;
    gap: 20px;
  }
  .mission-card {
    aspect-ratio: auto;
    min-height: 220px;
  }
  .card-glass {
    padding: 24px;
  }
  .mission-title {
    font-size: 20px;
    margin-bottom: 16px;
  }
}
</style>
