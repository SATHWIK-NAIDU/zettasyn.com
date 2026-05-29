<template>
  <main class="home">
    <Hero />
    <TeamSection />
    <FirmsSection />
    <ObjectivesSection />
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
  background: transparent;
  min-height: 100vh;
}
</style>