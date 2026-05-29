<template>
  <section id="hero" class="hero-section" ref="heroSection">
    <div class="hero-content">
      <div class="subtitle-wrapper" ref="subtitleWrapper">
        <span class="badge-dot" />
        <span class="hero-subtitle">The ZettaSyn Empire</span>
      </div>

      <h1 class="hero-heading" ref="heading">
        <div class="line"><SplitText text="Building the future" /></div>
        <div class="line"><SplitText text="through connected" /></div>
        <div class="line gradient-text"><SplitText text="ecosystems." /></div>
      </h1>

      <p class="hero-desc" ref="desc">
        A futuristic ecosystem powering production, esports, technology, 
        cinematic storytelling, digital infrastructure, and next-generation experiences.
      </p>

      <div class="hero-actions" ref="actions">
        <a href="#firms" class="magnetic-btn primary-btn" @click.prevent="scrollToSection('firms')" @mousemove="onBtnMove" @mouseleave="onBtnLeave">
          <span class="btn-text">Explore Ecosystem</span>
          <div class="btn-glow"></div>
        </a>
        <a href="#objectives" class="magnetic-btn glass-btn" @click.prevent="scrollToSection('objectives')" @mousemove="onBtnMove" @mouseleave="onBtnLeave">
          <span class="btn-text">Our Objectives</span>
          <div class="btn-glow"></div>
        </a>
        <a href="#contact" class="magnetic-btn ghost-btn" @click.prevent="scrollToSection('contact')" @mousemove="onBtnMove" @mouseleave="onBtnLeave">
          <span class="btn-text">Initialize Contact</span>
          <div class="btn-glow"></div>
        </a>
      </div>
    </div>
    
    <div class="scroll-indicator" ref="scrollIndicator">
      <div class="mouse">
        <div class="wheel"></div>
      </div>
      <span>Discover</span>
    </div>

    <!-- Tactical UI Indicators -->
    <div class="tactical-ui ui-top-left">
      <svg width="40" height="40" viewBox="0 0 100 100">
        <path d="M 10 10 L 30 10 M 10 10 L 10 30" stroke="rgba(255,255,255,0.2)" stroke-width="2" fill="none" />
        <circle cx="20" cy="20" r="2" fill="rgba(0,240,255,0.5)" />
      </svg>
      <div class="ui-data">SYS.RDY // 100%</div>
    </div>
    
    <div class="tactical-ui ui-bottom-right">
      <svg width="60" height="60" viewBox="0 0 100 100" class="rotating-ring">
        <circle cx="50" cy="50" r="40" stroke="rgba(0,240,255,0.1)" stroke-width="1" fill="none" stroke-dasharray="4 8" />
        <circle cx="50" cy="50" r="30" stroke="rgba(157,0,255,0.2)" stroke-width="1" fill="none" stroke-dasharray="10 10" />
      </svg>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import SplitText from '../ui/SplitText.vue'

if (process.client) {
  gsap.registerPlugin(ScrollTrigger)
}

const heroSection = ref(null)
const heading = ref(null)
const desc = ref(null)
const actions = ref(null)
const subtitleWrapper = ref(null)
const scrollIndicator = ref(null)

onMounted(() => {
  // We want to wait a tiny bit after the boot sequence, but since boot sequence is an overlay,
  // we can animate this in parallel with a delay, or better yet, trigger it via a store/event.
  // For now, we'll give it a solid delay assuming BootSequence takes ~5s
  
  const tl = gsap.timeline({ delay: 5.5 }) // 5.5s to wait for BootSequence

  tl.fromTo(subtitleWrapper.value, 
    { opacity: 0, y: 20 },
    { opacity: 1, y: 0, duration: 1, ease: 'power3.out' }
  )
  
  const lines = heading.value.querySelectorAll('.line')
  tl.fromTo(lines, 
    { opacity: 0, y: 40, rotationX: -20 },
    { opacity: 1, y: 0, rotationX: 0, duration: 1.2, stagger: 0.2, ease: 'power4.out', transformOrigin: '0% 50% -50' },
    "-=0.6"
  )

  tl.fromTo(desc.value,
    { opacity: 0, y: 20 },
    { opacity: 1, y: 0, duration: 1, ease: 'power3.out' },
    "-=0.8"
  )

  const btns = actions.value.querySelectorAll('.magnetic-btn')
  tl.fromTo(btns,
    { opacity: 0, scale: 0.9, y: 20 },
    { opacity: 1, scale: 1, y: 0, duration: 0.8, stagger: 0.1, ease: 'back.out(1.5)' },
    "-=0.6"
  )
  
  tl.fromTo(scrollIndicator.value,
    { opacity: 0 },
    { opacity: 1, duration: 1 },
    "-=0.4"
  )
  
  gsap.to('.wheel', {
    y: 8,
    opacity: 0,
    repeat: -1,
    duration: 1.5,
    ease: 'power2.inOut'
  })

  // Cinematic scroll-driven typography disassembly
  const chars = heading.value.querySelectorAll('.char')
  if (chars.length > 0) {
    gsap.to(chars, {
      scrollTrigger: {
        trigger: heroSection.value,
        start: 'top top',
        end: 'bottom top',
        scrub: 1.5,
      },
      y: () => (Math.random() - 0.5) * 200,
      x: () => (Math.random() - 0.5) * 200,
      rotationX: () => Math.random() * 90,
      rotationY: () => Math.random() * 90,
      opacity: 0,
      filter: 'blur(8px)',
      stagger: {
        amount: 0.8,
        from: 'random'
      },
      ease: 'power1.inOut'
    })
  }
})

function scrollToSection(id) {
  const el = document.getElementById(id)
  if (el) {
    const navbarHeight = 64
    const elementPosition = el.getBoundingClientRect().top + window.scrollY
    const offsetPosition = elementPosition - navbarHeight
    
    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    })
  }
}

function onBtnMove(e) {
  const btn = e.currentTarget
  const rect = btn.getBoundingClientRect()
  const x = e.clientX - rect.left - rect.width / 2
  const y = e.clientY - rect.top - rect.height / 2
  
  gsap.to(btn, {
    x: x * 0.3,
    y: y * 0.3,
    rotation: x * 0.05,
    duration: 0.4,
    ease: 'power3.out'
  })
  
  const glow = btn.querySelector('.btn-glow')
  if (glow) {
    gsap.to(glow, {
      x: e.clientX - rect.left - rect.width / 2,
      y: e.clientY - rect.top - rect.height / 2,
      duration: 0.1
    })
  }
}

function onBtnLeave(e) {
  const btn = e.currentTarget
  gsap.to(btn, {
    x: 0,
    y: 0,
    rotation: 0,
    duration: 0.7,
    ease: 'elastic.out(1, 0.3)'
  })
  
  const glow = btn.querySelector('.btn-glow')
  if (glow) {
    gsap.to(glow, { x: 0, y: 0, duration: 0.7 })
  }
}
</script>

<style scoped>
.hero-section {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0 24px;
  overflow: hidden;
  perspective: 1000px;
}

.hero-content {
  max-width: 1200px;
  text-align: center;
  z-index: 10;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.subtitle-wrapper {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  padding: 8px 24px;
  border-radius: 99px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  margin-bottom: 40px;
  box-shadow: 0 0 20px rgba(0, 240, 255, 0.1);
}

.badge-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--grad-to);
  box-shadow: 0 0 12px var(--grad-to);
  animation: pulse 2s infinite;
}

.hero-subtitle {
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 500;
  color: #e4e4e7;
  letter-spacing: 0.15em;
  text-transform: uppercase;
}

.hero-heading {
  font-family: var(--font-heading);
  font-size: clamp(48px, 8vw, 96px);
  font-weight: 700;
  line-height: 1.05;
  letter-spacing: -0.03em;
  color: #ffffff;
  margin-bottom: 32px;
}

.line {
  display: block;
}

/* Fix for background-clip: text breaking on transformed children */
.line.gradient-text {
  background: none;
  -webkit-text-fill-color: initial;
}

.line.gradient-text :deep(.char) {
  background: linear-gradient(135deg, var(--grad-from), var(--grad-to));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-desc {
  font-family: var(--font-body);
  font-weight: 300;
  font-size: clamp(16px, 2vw, 22px);
  line-height: 1.6;
  color: var(--text-secondary);
  max-width: 800px;
  margin-bottom: 56px;
}

.hero-actions {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 24px;
}

.magnetic-btn {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 16px 36px;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 500;
  font-family: var(--font-body);
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  transition: border-color 0.3s, background-color 0.3s, box-shadow 0.3s;
  z-index: 1;
}

.magnetic-btn .btn-text {
  position: relative;
  z-index: 2;
}

.btn-glow {
  position: absolute;
  width: 150%;
  height: 150%;
  background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 60%);
  top: -25%;
  left: -25%;
  z-index: 0;
  opacity: 0;
  transition: opacity 0.3s;
  pointer-events: none;
}

.magnetic-btn:hover .btn-glow {
  opacity: 1;
}

.primary-btn {
  background: linear-gradient(135deg, var(--grad-from), var(--grad-accent));
  color: #fff;
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 10px 30px rgba(28, 82, 246, 0.4);
}

.primary-btn:hover {
  box-shadow: 0 15px 40px rgba(28, 82, 246, 0.6), 0 0 20px var(--grad-to);
}

.glass-btn {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  color: #fff;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.glass-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.3);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
}

.ghost-btn {
  background: transparent;
  color: var(--text-secondary);
  border: 1px solid transparent;
}

.ghost-btn:hover {
  color: #fff;
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
}

.scroll-indicator {
  position: absolute;
  bottom: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  color: var(--text-muted);
  font-family: var(--font-body);
  font-size: 12px;
  text-transform: uppercase;
  letter-spacing: 0.2em;
}

.mouse {
  width: 26px;
  height: 40px;
  border: 2px solid var(--text-muted);
  border-radius: 20px;
  position: relative;
}

.wheel {
  position: absolute;
  top: 6px;
  left: 50%;
  transform: translateX(-50%);
  width: 4px;
  height: 8px;
  background: var(--text-muted);
  border-radius: 2px;
}

/* Tactical UI Indicators */
.tactical-ui {
  position: absolute;
  pointer-events: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  opacity: 0.6;
}

.ui-top-left {
  top: 100px;
  left: 40px;
}

.ui-bottom-right {
  bottom: 80px;
  right: 60px;
}

.ui-data {
  font-family: var(--font-body);
  font-size: 10px;
  color: var(--text-muted);
  letter-spacing: 0.2em;
  text-transform: uppercase;
}

.rotating-ring {
  animation: spin 20s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(0, 240, 255, 0.4); }
  70% { box-shadow: 0 0 0 10px rgba(0, 240, 255, 0); }
  100% { box-shadow: 0 0 0 0 rgba(0, 240, 255, 0); }
}

@media (max-width: 768px) {
  .hero-heading {
    font-size: clamp(36px, 10vw, 48px);
  }
  .hero-actions {
    flex-direction: column;
    width: 100%;
    max-width: 300px;
  }
}
</style>
