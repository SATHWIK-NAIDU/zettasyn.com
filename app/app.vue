<template>
  <div id="app-root">
    <ThreeBackground />
    <BootSequence v-if="showBootSequence" @complete="handleBootComplete" />
    
    <div class="main-content" :class="{ 'is-visible': !showBootSequence || bootComplete }">
      <Navbar />
      <NuxtPage />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed } from 'vue'
import { useRoute } from 'vue-router'
import Lenis from '@studio-freight/lenis'

const route = useRoute()
const bootComplete = ref(false)
const hasBooted = ref(false)

if (route.path !== '/') {
  hasBooted.value = true
  bootComplete.value = true
}

const showBootSequence = computed(() => {
  return route.path === '/' && !hasBooted.value
})

function handleBootComplete() {
  bootComplete.value = true
  hasBooted.value = true
}

let lenis

onMounted(() => {
  lenis = new Lenis({
    duration: 1.6,
    easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)), // Exponential ease-out
    direction: 'vertical',
    gestureDirection: 'vertical',
    smooth: true,
    wheelMultiplier: 1.1,
    mouseMultiplier: 1.1,
    smoothTouch: true, // Enable smooth scrolling on touch devices
    touchMultiplier: 2,
    infinite: false,
  })

  // Expose lenis globally for seamless cross-component scroll control!
  if (typeof window !== 'undefined') {
    window.lenis = lenis
  }

  function raf(time) {
    lenis.raf(time)
    requestAnimationFrame(raf)
  }

  requestAnimationFrame(raf)
})

onBeforeUnmount(() => {
  if (lenis) {
    lenis.destroy()
  }
})
</script>

<style scoped>
.main-content {
  opacity: 0;
  transition: opacity 1s ease-in-out;
  position: relative;
  z-index: 10;
}

.main-content.is-visible {
  opacity: 1;
}
</style>