<template>
  <section id="team" class="team-section" ref="teamSection">
    <div class="team-content">
      <h2 class="section-title" ref="title">
        <span class="gradient-text">Our Team</span>
      </h2>
      
      <div class="founders-grid" ref="grid">
        <div 
          v-for="(founder, index) in founders" 
          :key="index" 
          class="founder-card"
          @mousemove="onCardMove"
          @mouseleave="onCardLeave"
        >
          <div class="card-inner">
            <div class="holographic-border"></div>
            <div class="image-wrapper">
              <!-- Placeholder image, using a gradient for now -->
              <div class="placeholder-img"></div>
            </div>
            <div class="card-info">
              <h3 class="founder-name">{{ founder.name }}</h3>
              <p class="founder-role">{{ founder.role }}</p>
            </div>
          </div>
        </div>
      </div>

      <div class="explore-wrapper" ref="exploreWrapper">
        <button class="explore-btn" @click="expandTeam">
          <span>{{ isExpanded ? 'Hide Full Team' : 'Explore Full Team' }}</span>
          <div class="btn-line"></div>
        </button>
      </div>

      <!-- Expanded Team -->
      <div class="team-universe" :class="{ 'is-expanded': isExpanded }">
        <div class="founders-grid extended-grid">
          <div 
            v-for="(member, index) in extendedTeam" 
            :key="index" 
            class="founder-card universe-node"
            @mousemove="onCardMove"
            @mouseleave="onCardLeave"
          >
            <div class="card-inner">
              <div class="holographic-border"></div>
              <div class="image-wrapper">
                <div class="placeholder-img"></div>
              </div>
              <div class="card-info">
                <h3 class="founder-name">{{ member.name }}</h3>
                <p class="founder-role">{{ member.role }}</p>
              </div>
            </div>
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

const teamSection = ref(null)
const title = ref(null)
const grid = ref(null)
const exploreWrapper = ref(null)
const isExpanded = ref(false)

const founders = [
  { name: 'Founder One', role: 'Chief Executive Officer' },
  { name: 'Founder Two', role: 'Chief Operations Officer' },
  { name: 'Founder Three', role: 'Chief Technology Officer' },
  { name: 'Founder Four', role: 'Chief Creative Officer' }
]

const extendedTeam = [
  { name: 'Leader Five', role: 'Head of Media' },
  { name: 'Leader Six', role: 'Head of Infrastructure' },
  { name: 'Leader Seven', role: 'Creative Director' },
  { name: 'Leader Eight', role: 'Technical Director' }
]

onMounted(() => {
  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: teamSection.value,
      start: 'top 70%',
      end: 'bottom 20%',
      toggleActions: 'play none none reverse'
    }
  })

  tl.fromTo(title.value,
    { opacity: 0, y: 50, filter: 'blur(10px)' },
    { opacity: 1, y: 0, filter: 'blur(0px)', duration: 1, ease: 'power3.out' }
  )

  const cards = grid.value.querySelectorAll('.founder-card')
  tl.fromTo(cards,
    { opacity: 0, y: 100, rotationX: 15 },
    { opacity: 1, y: 0, rotationX: 0, duration: 1, stagger: 0.15, ease: 'power4.out', transformOrigin: '50% 100%' },
    "-=0.6"
  )

  tl.fromTo(exploreWrapper.value,
    { opacity: 0, scale: 0.9 },
    { opacity: 1, scale: 1, duration: 0.8, ease: 'back.out(1.5)' },
    "-=0.4"
  )
})

function onCardMove(e) {
  const card = e.currentTarget
  const inner = card.querySelector('.card-inner')
  const rect = card.getBoundingClientRect()
  const x = e.clientX - rect.left - rect.width / 2
  const y = e.clientY - rect.top - rect.height / 2
  
  // Tilt effect
  gsap.to(inner, {
    rotationY: x * 0.05,
    rotationX: -y * 0.05,
    z: 20,
    duration: 0.4,
    ease: 'power2.out'
  })
  
  // Holographic border response
  const border = card.querySelector('.holographic-border')
  gsap.to(border, {
    opacity: 1,
    background: `radial-gradient(circle at ${e.clientX - rect.left}px ${e.clientY - rect.top}px, rgba(0, 240, 255, 0.4) 0%, transparent 50%)`,
    duration: 0.2
  })
}

function onCardLeave(e) {
  const card = e.currentTarget
  const inner = card.querySelector('.card-inner')
  const border = card.querySelector('.holographic-border')
  
  gsap.to(inner, {
    rotationY: 0,
    rotationX: 0,
    z: 0,
    duration: 0.8,
    ease: 'elastic.out(1, 0.3)'
  })
  
  gsap.to(border, {
    opacity: 0.3,
    background: 'transparent',
    duration: 0.5
  })
}

function expandTeam() {
  if (!isExpanded.value) {
    isExpanded.value = true
    // Wait for display: block to apply before animating
    setTimeout(() => {
      gsap.fromTo('.universe-node', 
        { opacity: 0, scale: 0.8, y: 50 },
        { opacity: 1, scale: 1, y: 0, duration: 0.8, stagger: 0.1, ease: 'back.out(1.2)' }
      )
    }, 50)
  } else {
    gsap.to('.universe-node', {
      opacity: 0,
      scale: 0.8,
      y: 50,
      duration: 0.4,
      stagger: 0.05,
      ease: 'power2.in',
      onComplete: () => {
        isExpanded.value = false
      }
    })
  }
}
</script>

<style scoped>
.team-section {
  position: relative;
  padding: 160px 24px;
  background: transparent;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.team-content {
  max-width: 1200px;
  width: 100%;
  z-index: 2;
}

.section-title {
  font-family: var(--font-heading);
  font-size: clamp(40px, 6vw, 64px);
  font-weight: 700;
  text-align: center;
  margin-bottom: 80px;
  letter-spacing: -0.02em;
}

.founders-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 32px;
  margin-bottom: 80px;
  perspective: 1000px;
}

.founder-card {
  position: relative;
  width: 100%;
  aspect-ratio: 3/4;
  cursor: pointer;
  transform-style: preserve-3d;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  background: rgba(17, 17, 21, 0.6);
  border-radius: 16px;
  overflow: hidden;
  border: 1px solid var(--border-subtle);
  display: flex;
  flex-direction: column;
  transform-style: preserve-3d;
  transition: box-shadow 0.3s;
}

.founder-card:hover .card-inner {
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.6), 0 0 20px rgba(0, 240, 255, 0.1);
}

.holographic-border {
  position: absolute;
  inset: 0;
  border-radius: 16px;
  opacity: 0.3;
  z-index: 10;
  pointer-events: none;
  transition: opacity 0.3s;
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.1);
}

.image-wrapper {
  flex: 1;
  width: 100%;
  overflow: hidden;
  position: relative;
}

.placeholder-img {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #1c1c22, #2a2a35);
  transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.founder-card:hover .placeholder-img {
  transform: scale(1.05);
}

.card-info {
  padding: 24px;
  position: relative;
  z-index: 5;
  background: linear-gradient(to top, rgba(10, 10, 12, 1) 40%, rgba(10, 10, 12, 0) 100%);
  transform: translateZ(10px);
}

.founder-name {
  font-family: var(--font-heading);
  font-size: 20px;
  color: #fff;
  margin-bottom: 4px;
  transition: color 0.3s;
}

.founder-card:hover .founder-name {
  color: var(--grad-to);
}

.founder-role {
  font-family: var(--font-body);
  font-size: 13px;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.explore-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 60px;
}

.explore-btn {
  background: transparent;
  border: none;
  color: #fff;
  font-family: var(--font-body);
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
}

.btn-line {
  width: 100%;
  height: 1px;
  background: linear-gradient(to right, transparent, var(--grad-to), transparent);
  transform: scaleX(0.3);
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.explore-btn:hover .btn-line {
  transform: scaleX(1);
}

.team-universe {
  display: none;
  opacity: 0;
  transition: opacity 0.5s;
}

.team-universe.is-expanded {
  display: block;
  opacity: 1;
}

.extended-grid {
  padding-top: 40px;
  border-top: 1px solid var(--border-subtle);
  background: radial-gradient(circle at top, rgba(28, 82, 246, 0.05) 0%, transparent 60%);
}

.universe-node {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.node-dot {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid var(--border-subtle);
  box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.5);
  position: relative;
  transition: all 0.3s;
}

.node-dot::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: var(--grad-accent);
  box-shadow: 0 0 10px var(--grad-accent);
}

.universe-node:hover .node-dot {
  border-color: var(--grad-to);
  box-shadow: 0 0 20px rgba(0, 240, 255, 0.2);
}

.node-label {
  font-family: var(--font-body);
  font-size: 12px;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.1em;
}
</style>
