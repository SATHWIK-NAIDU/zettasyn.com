<template>
  <div class="boot-sequence" :class="{ 'is-hidden': isComplete }" ref="bootOverlay">
    
    <div class="boot-content">
      <div class="message" ref="messageEl">
        <SplitText text="POWER OF A MILLION MINDS CONNECTED BY SYNERGY" />
      </div>

      <div class="logo-container" ref="logoContainerEl">
        <div class="logo-text-wrapper" ref="logoTextEl">
          <SplitText text="ZettaSyn" />
        </div>
      </div>
    </div>

    <!-- Skip Button -->
    <button class="skip-btn" ref="skipBtn" @click="skipSequence">
      SKIP.INIT [ESC]
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import gsap from 'gsap'
import SplitText from '../ui/SplitText.vue'

const emit = defineEmits(['complete'])
const isComplete = ref(false)
let mainTimeline = null

const bootOverlay = ref(null)
const messageEl = ref(null)
const logoContainerEl = ref(null)
const logoTextEl = ref(null)
const skipBtn = ref(null)

function skipSequence() {
  if (isComplete.value) return
  if (mainTimeline) mainTimeline.kill()
  
  isComplete.value = true
  document.body.style.overflow = ''
  sessionStorage.setItem('zettasyn_booted', 'true')
  
  gsap.set(bootOverlay.value, { opacity: 0, display: 'none' })
  emit('complete')
}

function handleKeydown(e) {
  if (e.key === 'Escape') skipSequence()
}

onMounted(() => {
  // Check if already booted in this session
  if (sessionStorage.getItem('zettasyn_booted')) {
    isComplete.value = true
    return emit('complete')
  }

  document.body.style.overflow = 'hidden'
  window.addEventListener('keydown', handleKeydown)

  // Get characters for animation
  const taglineChars = messageEl.value.querySelectorAll('.char')
  const logoChars = logoContainerEl.value.querySelectorAll('.char')

  mainTimeline = gsap.timeline({
    onComplete: () => {
      skipSequence()
    }
  })

  // Set initial states
  gsap.set(taglineChars, { opacity: 0, z: -50, filter: 'blur(10px)' })
  gsap.set(logoContainerEl.value, { display: 'none' })
  gsap.set(skipBtn.value, { opacity: 0, pointerEvents: 'none' })

  // Fade in skip button
  mainTimeline.to(skipBtn.value, { opacity: 0.5, pointerEvents: 'all', duration: 1 }, 2)

  // ==========================================
  // PHASE 1 — TAGLINE INITIALIZATION (5s)
  // ==========================================
  mainTimeline.to(taglineChars, {
    opacity: 1,
    z: 0,
    filter: 'blur(0px)',
    duration: 1.5,
    stagger: { amount: 1.5, from: 'random' },
    ease: 'power3.out'
  }, 0.5)

  // Hold and breathe
  mainTimeline.to(taglineChars, {
    scale: 1.02,
    duration: 2,
    ease: 'sine.inOut'
  }, "-=1")

  // ==========================================
  // PHASE 2 — GLITCH TRANSITION
  // ==========================================
  mainTimeline.to(taglineChars, {
    opacity: 0,
    filter: 'blur(20px)',
    scale: 1.1,
    duration: 0.4,
    stagger: { amount: 0.2, from: 'center' },
    ease: 'power4.in'
  }, "+=0.5")

  // ==========================================
  // PHASE 3A — LOGO FORMATION (2s)
  // ==========================================
  mainTimeline.set(logoContainerEl.value, { display: 'flex', opacity: 1 })
  mainTimeline.set(logoChars, { opacity: 0, y: 30, rotationX: 90, filter: 'blur(10px)' })

  mainTimeline.to(logoChars, {
    opacity: 1,
    y: 0,
    rotationX: 0,
    filter: 'blur(0px)',
    duration: 1.5,
    stagger: 0.1,
    ease: 'expo.out'
  })

  // ==========================================
  // PHASE 3B — STABLE DISPLAY (3s)
  // ==========================================
  mainTimeline.to(logoTextEl.value, {
    filter: 'drop-shadow(0 0 40px rgba(0, 240, 255, 0.6))',
    duration: 1.5,
    yoyo: true,
    repeat: 1,
    ease: 'sine.inOut'
  })

  // ==========================================
  // PHASE 3C — DISMANTLING (2s)
  // ==========================================
  mainTimeline.to(logoChars, {
    opacity: 0,
    y: -50,
    rotationX: -90,
    z: 100,
    filter: 'blur(15px)',
    duration: 1.2,
    stagger: { amount: 0.5, from: 'center' },
    ease: 'power3.in'
  })

  // ==========================================
  // FINAL TRANSITION
  // ==========================================
  mainTimeline.to(bootOverlay.value, {
    opacity: 0,
    duration: 1.5,
    ease: 'power2.inOut'
  }, "-=0.5")
})

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeydown)
  if (mainTimeline) mainTimeline.kill()
})
</script>

<style scoped>
.boot-sequence {
  position: fixed;
  inset: 0;
  z-index: 9999;
  background: rgba(5, 5, 5, 0.85); /* Semi-transparent to let ThreeBackground shine through */
  backdrop-filter: blur(4px);
  display: flex;
  justify-content: center;
  align-items: center;
  pointer-events: all;
  perspective: 1000px;
}

.boot-sequence.is-hidden {
  display: none;
}

.boot-content {
  position: relative;
  text-align: center;
  width: 100%;
  max-width: 800px;
}

.message {
  font-family: var(--font-body);
  font-size: clamp(14px, 3vw, 24px);
  color: var(--text-primary);
  font-weight: 300;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  text-shadow: 0 0 15px rgba(255, 255, 255, 0.3);
  /* The opacity is controlled by GSAP, but ensure block formatting */
  display: block;
}

.logo-container {
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: absolute;
  inset: 0;
  opacity: 0;
  transform-style: preserve-3d;
}

.logo-text-wrapper {
  font-family: var(--font-heading);
  font-size: clamp(40px, 8vw, 80px);
  font-weight: 700;
  color: #fff;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  transform-style: preserve-3d;
}

.skip-btn {
  position: absolute;
  bottom: 40px;
  right: 40px;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: var(--text-muted);
  font-family: var(--font-body);
  font-size: 10px;
  letter-spacing: 0.2em;
  padding: 8px 16px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.skip-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border-color: rgba(255, 255, 255, 0.5);
}
</style>
