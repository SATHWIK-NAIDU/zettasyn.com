<template>
  <nav :class="['navbar', { 'navbar--scrolled': scrolled, 'navbar--open': menuOpen }]">
    <div class="navbar__inner">

      <!-- Wordmark -->
      <a href="#about" class="navbar__wordmark" @click.prevent="scrollToTop">
        <img src="/logo.png" alt="Zettasyn Logo" class="navbar__logo-img" />
      </a>

      <!-- Desktop links -->
      <div ref="linksContainer" class="navbar__links">
        <div :style="highlighterStyle" class="navbar__highlighter" />
        
        <a
          v-for="(item, idx) in navItems"
          :key="item.label"
          :href="item.href"
          :class="['navbar__link', { 'navbar__link--active': activeIdx === idx }]"
          @click.prevent="scrollToSection(item.href, idx)"
          :ref="el => { if (el) linkRefs[idx] = el }"
        >
          {{ item.label }}
        </a>
      </div>

      <!-- Mobile hamburger -->
      <button
        class="navbar__hamburger"
        @click="menuOpen = !menuOpen"
        aria-label="Toggle menu"
      >
        <span :class="{ 'bar--top-open': menuOpen }" class="bar" />
        <span :class="{ 'bar--mid-open': menuOpen }" class="bar" />
        <span :class="{ 'bar--bot-open': menuOpen }" class="bar" />
      </button>

    </div>

    <!-- Mobile Drawer -->
    <Transition name="drawer">
      <div v-if="menuOpen" class="navbar__drawer">
        <a
          v-for="(item, idx) in navItems"
          :key="item.label"
          :href="item.href"
          :class="['drawer__link', { 'drawer__link--active': activeIdx === idx }]"
          @click.prevent="scrollToSection(item.href, idx); menuOpen = false"
        >
          {{ item.label }}
        </a>
      </div>
    </Transition>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch, nextTick } from 'vue'

const navItems = [
  { label: 'Team',       href: '#team' },
  { label: 'Firms',      href: '#firms' },
  { label: 'Objectives', href: '#objectives' },
  { label: 'Careers',    href: '#careers' },
  { label: 'Contact',    href: '#contact' },
]

const scrolled = ref(false)
const menuOpen = ref(false)
const activeIdx = ref(-1)
const linkRefs = ref([])
const linksContainer = ref(null)

const highlighterStyle = ref({
  left: '0px',
  width: '0px',
  height: '0px',
  top: '0px',
  opacity: 0,
})

function onScroll() {
  scrolled.value = window.scrollY > 40
}

function scrollToTop() {
  if (typeof window !== 'undefined' && window.lenis) {
    window.lenis.scrollTo(0, {
      duration: 1.6,
      easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t))
    })
  } else {
    window.scrollTo({
      top: 0,
      behavior: 'smooth'
    })
  }
  activeIdx.value = -1
}

function scrollToSection(href, idx) {
  const id = href.replace('#', '')
  const el = document.getElementById(id)
  if (el) {
    if (typeof window !== 'undefined' && window.lenis) {
      window.lenis.scrollTo(el, {
        offset: -64,
        duration: 1.6,
        easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t))
      })
    } else {
      const navbarHeight = 64
      const elementPosition = el.getBoundingClientRect().top + window.scrollY
      const offsetPosition = elementPosition - navbarHeight
      
      window.scrollTo({
        top: offsetPosition,
        behavior: 'smooth'
      })
    }
    
    activeIdx.value = idx
  }
}

function updateHighlighter() {
  const activeEl = linkRefs.value[activeIdx.value]
  const containerEl = linksContainer.value
  if (activeEl && containerEl) {
    const activeRect = activeEl.getBoundingClientRect()
    const containerRect = containerEl.getBoundingClientRect()
    
    highlighterStyle.value = {
      left: `${activeRect.left - containerRect.left}px`,
      width: `${activeRect.width}px`,
      height: `${activeRect.height}px`,
      top: `${activeEl.offsetTop}px`,
      opacity: 1,
    }
  } else {
    highlighterStyle.value = {
      left: '0px',
      width: '0px',
      height: '0px',
      top: '0px',
      opacity: 0,
    }
  }
}

watch(activeIdx, () => {
  nextTick(() => {
    updateHighlighter()
  })
})

let observer = null

onMounted(() => {
  window.addEventListener('scroll', onScroll)
  window.addEventListener('resize', updateHighlighter)
  
  onScroll()
  
  setTimeout(() => {
    updateHighlighter()
  }, 150)

  // Intersection Observer for scroll tracking
  const sections = ['hero', 'team', 'firms', 'objectives', 'careers', 'contact']
  const observerOptions = {
    root: null,
    rootMargin: '-30% 0px -50% 0px',
    threshold: 0.05,
  }

  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        const id = entry.target.getAttribute('id')
        if (id === 'hero') {
          activeIdx.value = -1
        } else {
          const idx = navItems.findIndex(item => item.href === `#${id}`)
          if (idx !== -1) {
            activeIdx.value = idx
          }
        }
      }
    })
  }, observerOptions)

  sections.forEach((id) => {
    const el = document.getElementById(id)
    if (el) observer.observe(el)
  })
})

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('resize', updateHighlighter)
  if (observer) {
    observer.disconnect()
  }
})
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  background: rgba(9, 9, 11, 0.4);
  border-bottom: 1px solid rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.navbar--scrolled {
  background: rgba(9, 9, 11, 0.8);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4), inset 0 1px 0 rgba(255, 255, 255, 0.05);
}

.navbar__inner {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 24px;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.navbar__wordmark {
  display: flex;
  align-items: center;
  text-decoration: none;
  transition: opacity 0.2s ease;
}

.navbar__wordmark:hover {
  opacity: 0.85;
}

.navbar__logo-img {
  height: 150px;
  width: auto;
  display: block;
  object-fit: contain;
}

.navbar__links {
  display: flex;
  align-items: center;
  gap: 4px;
  position: relative;
  padding: 4px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 99px;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.03);
}

.navbar__highlighter {
  position: absolute;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 99px;
  transition: all 0.35s cubic-bezier(0.16, 1, 0.3, 1);
  pointer-events: none;
  z-index: 0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25), inset 0 1px 0 rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(4px);
}

/* Subtle glowing bottom line on the sliding capsule */
.navbar__highlighter::after {
  content: '';
  position: absolute;
  bottom: 0px;
  left: 20%;
  right: 20%;
  height: 2px;
  background: linear-gradient(90deg, #a855f7, #ec4899);
  border-radius: 99px;
  box-shadow: 0 1px 8px rgba(168, 85, 247, 0.5);
  opacity: 0.8;
}

.navbar__link {
  font-size: 14px;
  font-weight: 500;
  color: #a1a1aa;
  text-decoration: none;
  padding: 8px 18px;
  border-radius: 99px;
  transition: color 0.3s ease;
  position: relative;
  z-index: 1;
}

.navbar__link:hover {
  color: #ffffff;
}

.navbar__link--active {
  color: #ffffff;
}

/* Hamburger */
.navbar__hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  z-index: 101;
}

.bar {
  display: block;
  width: 22px;
  height: 2px;
  background: #fff;
  border-radius: 2px;
  transition: transform 0.25s ease, opacity 0.25s ease;
}

.bar--top-open { transform: rotate(45deg) translate(5px, 5px); }
.bar--mid-open { opacity: 0; }
.bar--bot-open { transform: rotate(-45deg) translate(5px, -5px); }

/* Mobile drawer */
.navbar__drawer {
  position: fixed;
  top: 64px; left: 0; right: 0; bottom: 0;
  background: rgba(9, 9, 11, 0.96);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  display: flex;
  flex-direction: column;
  padding: 32px 24px;
  gap: 0;
  z-index: 99;
}

.drawer__link {
  font-size: 22px;
  font-weight: 500;
  color: #a1a1aa;
  text-decoration: none;
  padding: 16px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.drawer__link:hover,
.drawer__link--active {
  color: #ffffff;
}

.drawer__link--active::after {
  content: '→';
  font-weight: bold;
  color: #a855f7; /* Vibrant purple brand color */
}

/* Drawer transition */
.drawer-enter-active,
.drawer-leave-active { transition: opacity 0.25s ease, transform 0.25s ease; }
.drawer-enter-from,
.drawer-leave-to    { opacity: 0; transform: translateY(-8px); }

/* Responsive */
@media (max-width: 768px) {
  .navbar {
    background: #09090b !important; /* Fully solid dark background for mobile ratio */
    border-bottom: 1px solid rgba(255, 255, 255, 0.08) !important;
    backdrop-filter: none !important;
    -webkit-backdrop-filter: none !important;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5) !important;
  }
  
  .navbar--scrolled {
    background: #09090b !important;
  }

  .navbar--open {
    background: #09090b !important;
    border-bottom: 1px solid rgba(255, 255, 255, 0.05) !important;
    box-shadow: none !important;
  }

  .navbar__links    { display: none; }
  .navbar__hamburger { display: flex; }
}
</style>