<template>
  <div class="legal-page">
    <div class="legal-header">
      <NuxtLink to="/" class="back-link">← Back to Ecosystem</NuxtLink>
      <h1 class="page-title">Terms of Service</h1>
      <p class="last-updated">Last Updated: {{ new Date().toLocaleDateString() }}</p>
    </div>
    
    <div class="legal-content">
      <section>
        <h2>1. Acceptance of Terms</h2>
        <p>By accessing and using the Zettasyn platform, ecosystem, and related services, you agree to be bound by these Terms of Service. If you do not agree with any part of these terms, you must not use our services.</p>
      </section>

      <section>
        <h2>2. Intellectual Property Rights</h2>
        <p>All content, features, and functionality (including but not limited to all information, software, text, displays, images, video, and audio) are owned by Zettasyn Pvt Ltd, its licensors, or other providers of such material and are protected by international copyright, trademark, patent, trade secret, and other intellectual property laws.</p>
      </section>

      <section>
        <h2>3. User Responsibilities</h2>
        <p>You agree to use our services only for lawful purposes and in accordance with these Terms. You are responsible for ensuring that your use of the services does not violate any applicable local, state, national, or international laws.</p>
      </section>

      <section>
        <h2>4. Limitation of Liability</h2>
        <p>In no event shall Zettasyn Pvt Ltd, nor its directors, employees, partners, agents, suppliers, or affiliates, be liable for any indirect, incidental, special, consequential or punitive damages, including without limitation, loss of profits, data, use, goodwill, or other intangible losses, resulting from your access to or use of or inability to access or use the service.</p>
      </section>

      <section>
        <h2>5. Modifications to Service</h2>
        <p>Zettasyn reserves the right to modify or discontinue, temporarily or permanently, the service (or any part thereof) with or without notice. You agree that Zettasyn shall not be liable to you or to any third party for any modification, suspension, or discontinuance of the service.</p>
      </section>

      <section>
        <h2>6. Governing Law</h2>
        <p>These Terms shall be governed and construed in accordance with the laws of the jurisdiction in which Zettasyn operates, without regard to its conflict of law provisions.</p>
      </section>
    </div>
    
    <Footer />
  </div>
</template>

<script setup>
import Footer from '~/components/sections/Footer.vue'
</script>

<style scoped>
.legal-page {
  background: var(--bg-primary, #050505);
  min-height: 100vh;
  color: var(--text-primary, #fff);
  padding-top: 80px;
}

.legal-header {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px 24px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.back-link {
  display: inline-block;
  color: var(--grad-to, #00f0ff);
  text-decoration: none;
  font-family: var(--font-body, sans-serif);
  font-size: 14px;
  margin-bottom: 24px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  transition: opacity 0.2s;
}

.back-link:hover {
  opacity: 0.8;
}

.page-title {
  font-family: var(--font-heading, sans-serif);
  font-size: clamp(32px, 5vw, 48px);
  margin-bottom: 16px;
}

.last-updated {
  font-family: var(--font-body, sans-serif);
  color: var(--text-secondary, #a1a1aa);
  font-size: 14px;
}

.legal-content {
  max-width: 800px;
  margin: 0 auto;
  padding: 64px 24px 120px 24px;
}

.legal-content section {
  margin-bottom: 48px;
}

.legal-content h2 {
  font-family: var(--font-heading, sans-serif);
  font-size: 24px;
  margin-bottom: 16px;
  color: #fff;
}

.legal-content p {
  font-family: var(--font-body, sans-serif);
  font-size: 16px;
  line-height: 1.7;
  color: var(--text-secondary, #a1a1aa);
  margin-bottom: 16px;
}
</style>
