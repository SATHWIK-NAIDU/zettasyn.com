<template>
  <div class="legal-page">
    <div class="legal-header">
      <NuxtLink to="/" class="back-link">← Back to Ecosystem</NuxtLink>
      <h1 class="page-title">Privacy Policy</h1>
      <p class="last-updated">Last Updated: {{ new Date().toLocaleDateString() }}</p>
    </div>
    
    <div class="legal-content">
      <section>
        <h2>1. Information We Collect</h2>
        <p>Zettasyn collects information that you provide directly to us when you use our platform, create an account, subscribe to our communications, or communicate with us. This may include personal identifiers, contact data, and professional information.</p>
        <p>We also automatically collect certain information when you interact with our systems, including log data, device information, and usage patterns.</p>
      </section>

      <section>
        <h2>2. How We Use Your Information</h2>
        <p>We use the collected information for the following purposes:</p>
        <ul>
          <li>To provide, maintain, and improve our services and ecosystem architecture.</li>
          <li>To communicate with you regarding updates, security alerts, and administrative messages.</li>
          <li>To analyze trends and optimize the user experience across our digital properties.</li>
          <li>To comply with applicable legal requirements and industry standards.</li>
        </ul>
      </section>

      <section>
        <h2>3. Data Sharing and Disclosure</h2>
        <p>We do not sell your personal information. We may share your data with trusted third-party service providers who operate under strict confidentiality agreements to assist us in running our infrastructure. We may also disclose information if required by law or to protect the rights and safety of Zettasyn and its users.</p>
      </section>

      <section>
        <h2>4. Data Security</h2>
        <p>Zettasyn implements enterprise-grade security measures designed to protect your personal information from unauthorized access, alteration, disclosure, or destruction. While we strive to use commercially acceptable means to protect your data, no method of transmission over the Internet is 100% secure.</p>
      </section>

      <section>
        <h2>5. Your Privacy Rights</h2>
        <p>Depending on your jurisdiction, you may have the right to access, correct, delete, or restrict the processing of your personal data. To exercise these rights, please contact our privacy compliance team via the secure channels provided in our command center.</p>
      </section>

      <section>
        <h2>6. Changes to This Policy</h2>
        <p>We may update this Privacy Policy from time to time to reflect changes in our practices or regulatory requirements. We will notify you of any material changes by posting the updated policy on this page.</p>
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

.legal-content p,
.legal-content li {
  font-family: var(--font-body, sans-serif);
  font-size: 16px;
  line-height: 1.7;
  color: var(--text-secondary, #a1a1aa);
  margin-bottom: 16px;
}

.legal-content ul {
  padding-left: 24px;
  margin-bottom: 16px;
}

.legal-content li {
  margin-bottom: 8px;
}
</style>
