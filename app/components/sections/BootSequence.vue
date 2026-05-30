<template>
  <div class="boot-sequence" :class="{ 'is-hidden': isComplete }" ref="bootOverlay">
    
    <!-- Background Animation Elements -->
    <div class="boot-background">
      <div class="grid-overlay"></div>
      <div class="particles">
        <div v-for="n in 18" :key="n" class="particle" :class="'p-' + n"></div>
      </div>
    </div>

    <div class="boot-content">
      <div class="message" ref="messageEl">
        <SplitText text="POWER OF A MILLION MINDS CONNECTED BY SYNERGY" />
      </div>

      <div class="logo-container" ref="logoContainerEl">
        <div class="logo-img-wrapper" ref="logoImgWrapperEl">
          <img src="/logo.png" alt="Zettasyn Logo" class="boot-logo-img" draggable="false" />
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
const logoImgWrapperEl = ref(null)
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
  mainTimeline.set(logoImgWrapperEl.value, { opacity: 0, scale: 0.85, rotationX: 45, filter: 'blur(20px)', z: -100 })

  mainTimeline.to(logoImgWrapperEl.value, {
    opacity: 1,
    scale: 1,
    rotationX: 0,
    z: 0,
    filter: 'blur(0px)',
    duration: 1.8,
    ease: 'expo.out'
  })

  // ==========================================
  // PHASE 3B — STABLE DISPLAY (3s)
  // ==========================================
  mainTimeline.to(logoImgWrapperEl.value, {
    filter: 'drop-shadow(0 0 45px rgba(0, 240, 255, 0.55))',
    scale: 1.03,
    duration: 1.5,
    yoyo: true,
    repeat: 1,
    ease: 'sine.inOut'
  })

  // ==========================================
  // PHASE 3C — DISMANTLING (2s)
  // ==========================================
  mainTimeline.to(logoImgWrapperEl.value, {
    opacity: 0,
    y: -40,
    rotationX: -45,
    z: 100,
    filter: 'blur(15px)',
    duration: 1.2,
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
  background: rgba(5, 5, 5, 0.88); /* Semi-transparent to let ThreeBackground shine through */
  backdrop-filter: blur(6px);
  display: flex;
  justify-content: center;
  align-items: center;
  pointer-events: all;
  perspective: 1000px;
  overflow: hidden;
}

.boot-sequence.is-hidden {
  display: none;
}

/* Background animation elements */
.boot-background {
  position: absolute;
  inset: 0;
  overflow: hidden;
  z-index: 0;
  pointer-events: none;
}

.grid-overlay {
  position: absolute;
  inset: 0;
  background-image: 
    linear-gradient(rgba(0, 240, 255, 0.02) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0, 240, 255, 0.02) 1px, transparent 1px);
  background-size: 50px 50px;
  background-position: center;
  mask-image: radial-gradient(circle at center, black 30%, transparent 80%);
  -webkit-mask-image: radial-gradient(circle at center, black 30%, transparent 80%);
  animation: gridPulse 8s infinite ease-in-out;
}

@keyframes gridPulse {
  0%, 100% {
    opacity: 0.4;
    transform: scale(1);
  }
  50% {
    opacity: 0.8;
    transform: scale(1.04);
  }
}

.particles {
  position: absolute;
  inset: 0;
}

.particle {
  position: absolute;
  border-radius: 50%;
  background: radial-gradient(circle at center, rgba(0, 240, 255, 0.35) 0%, transparent 70%);
  filter: blur(1px);
  opacity: 0;
  animation: floatParticle 9s infinite linear;
}

/* Float particles setup */
.p-1 { left: 10%; top: 20%; width: 6px; height: 6px; animation-delay: 0s; animation-duration: 9s; }
.p-2 { left: 25%; top: 75%; width: 4px; height: 4px; animation-delay: 1.5s; animation-duration: 7s; }
.p-3 { left: 40%; top: 15%; width: 8px; height: 8px; animation-delay: 3s; animation-duration: 11s; }
.p-4 { left: 55%; top: 80%; width: 5px; height: 5px; animation-delay: 0.5s; animation-duration: 8s; }
.p-5 { left: 70%; top: 30%; width: 7px; height: 7px; animation-delay: 2s; animation-duration: 10s; }
.p-6 { left: 85%; top: 65%; width: 4px; height: 4px; animation-delay: 4s; animation-duration: 6s; }
.p-7 { left: 15%; top: 60%; width: 5px; height: 5px; animation-delay: 1s; animation-duration: 8.5s; }
.p-8 { left: 30%; top: 40%; width: 9px; height: 9px; animation-delay: 2.5s; animation-duration: 12s; }
.p-9 { left: 50%; top: 50%; width: 6px; height: 6px; animation-delay: 3.5s; animation-duration: 9.5s; }
.p-10 { left: 65%; top: 10%; width: 4px; height: 4px; animation-delay: 0.2s; animation-duration: 7.5s; }
.p-11 { left: 80%; top: 85%; width: 8px; height: 8px; animation-delay: 1.8s; animation-duration: 10.5s; }
.p-12 { left: 90%; top: 25%; width: 5px; height: 5px; animation-delay: 2.8s; animation-duration: 8s; }
.p-13 { left: 5%; top: 85%; width: 7px; height: 7px; animation-delay: 4.2s; animation-duration: 11s; }
.p-14 { left: 35%; top: 90%; width: 4px; height: 4px; animation-delay: 0.7s; animation-duration: 6.5s; }
.p-15 { left: 45%; top: 5%; width: 6px; height: 6px; animation-delay: 2.3s; animation-duration: 9s; }
.p-16 { left: 75%; top: 55%; width: 5px; height: 5px; animation-delay: 1.3s; animation-duration: 8s; }
.p-17 { left: 20%; top: 5%; width: 8px; height: 8px; animation-delay: 3.7s; animation-duration: 12s; }
.p-18 { left: 60%; top: 95%; width: 6px; height: 6px; animation-delay: 0.9s; animation-duration: 7.5s; }

@keyframes floatParticle {
  0% {
    transform: translateY(120px) scale(0.4);
    opacity: 0;
  }
  15% {
    opacity: 0.7;
  }
  85% {
    opacity: 0.7;
  }
  100% {
    transform: translateY(-180px) scale(1.3);
    opacity: 0;
  }
}

.boot-content {
  position: relative;
  z-index: 2;
  text-align: center;
  width: 100%;
  max-width: 800px;
}

.message {
  font-family: var(--font-body);
  font-size: clamp(13px, 2.5vw, 20px);
  color: var(--text-primary);
  font-weight: 300;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  text-shadow: 0 0 15px rgba(255, 255, 255, 0.35);
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

.logo-img-wrapper {
  transform-style: preserve-3d;
  display: flex;
  align-items: center;
  justify-content: center;
  max-width: 90%;
}

.boot-logo-img {
  width: 100%;
  max-width: 420px; /* Striking massive brand appearance */
  height: auto;
  display: block;
  object-fit: contain;
  pointer-events: none;
}

.skip-btn {
  position: absolute;
  bottom: 40px;
  right: 40px;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.15);
  color: var(--text-muted);
  font-family: var(--font-body);
  font-size: 10px;
  letter-spacing: 0.2em;
  padding: 8px 16px;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 5;
}

.skip-btn:hover {
  background: rgba(255, 255, 255, 0.08);
  color: #fff;
  border-color: rgba(255, 255, 255, 0.4);
}
</style>
