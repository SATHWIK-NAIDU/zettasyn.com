<template>
  <main class="home">
    <Hero />
    <Ecosystem />
    <Backbone />
    <CareersSection />
    <ContactSection />
    <Footer />
  </main>
</template>

<script setup>
useHead({
  title: 'Zettasyn Pvt Ltd — Centralized Infrastructure for Creative Industries',
})
</script>

<style scoped>
.home {
  background: var(--bg-primary);
  min-height: 100vh;
}
</style>