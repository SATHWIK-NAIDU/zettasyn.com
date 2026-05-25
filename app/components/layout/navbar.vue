<template>
  <nav :class="['navbar', { 'navbar--scrolled': scrolled }]">
    <div class="navbar__inner">

      <!-- Wordmark -->
      <NuxtLink to="/" class="navbar__wordmark">
        ZETTASYN
      </NuxtLink>

      <!-- Desktop links -->
      <div class="navbar__links">
        <NuxtLink
          v-for="item in navItems"
          :key="item.label"
          :to="item.href"
          class="navbar__link"
        >
          {{ item.label }}
        </NuxtLink>
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
        <NuxtLink
          v-for="item in navItems"
          :key="item.label"
          :to="item.href"
          class="drawer__link"
          @click="menuOpen = false"
        >
          {{ item.label }}
        </NuxtLink>
      </div>
    </Transition>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const navItems = [
  { label: 'About',     href: '/about' },
  { label: 'Ecosystem', href: '/ecosystem' },
  { label: 'Careers',   href: '/careers' },
  { label: 'Contact',   href: '/contact' },
]

const scrolled  = ref(false)
const menuOpen  = ref(false)

function onScroll() {
  scrolled.value = window.scrollY > 40
}

onMounted(() => window.addEventListener('scroll', onScroll))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  border-bottom: 1px solid transparent;
  transition: background 0.3s ease, border-color 0.3s ease, backdrop-filter 0.3s ease;
}

.navbar--scrolled {
  background: rgba(9, 9, 11, 0.92);
  border-color: var(--border-subtle);
  backdrop-filter: blur(12px);
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
  font-weight: 700;
  font-size: 18px;
  letter-spacing: 0.08em;
  color: #fff;
  text-decoration: none;
}

.navbar__links {
  display: flex;
  align-items: center;
  gap: 36px;
}

.navbar__link {
  font-size: 15px;
  font-weight: 400;
  color: var(--text-muted);
  text-decoration: none;
  transition: color 0.2s ease;
}

.navbar__link:hover,
.navbar__link.router-link-active {
  color: var(--text-primary);
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
  background: var(--bg-primary);
  border-top: 1px solid var(--border-subtle);
  display: flex;
  flex-direction: column;
  padding: 32px 24px;
  gap: 0;
  z-index: 99;
}

.drawer__link {
  font-size: 24px;
  font-weight: 500;
  color: var(--text-secondary);
  text-decoration: none;
  padding: 16px 0;
  border-bottom: 1px solid var(--bg-surface);
  transition: color 0.2s ease;
}

.drawer__link:hover { color: #fff; }

/* Drawer transition */
.drawer-enter-active,
.drawer-leave-active { transition: opacity 0.25s ease, transform 0.25s ease; }
.drawer-enter-from,
.drawer-leave-to    { opacity: 0; transform: translateY(-8px); }

/* Responsive */
@media (max-width: 768px) {
  .navbar__links    { display: none; }
  .navbar__hamburger { display: flex; }
}
</style>