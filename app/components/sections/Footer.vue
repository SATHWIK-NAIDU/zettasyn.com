<template>
  <footer class="footer-section">
    <div class="footer-container">
      <!-- Top Grid -->
      <div class="footer-grid">
        <!-- Brand Column -->
        <div class="brand-column">
          <span class="brand-name">Zettasyn</span>
          <p class="brand-desc">
            Centralized operational, financial, and technical infrastructure powering the next generation of creative industries and digital ventures.
          </p>
          <!-- Social Icons Placeholder -->
          <div class="social-icons">
            <a 
              v-for="(icon, idx) in socialIcons" 
              :key="idx" 
              href="#" 
              class="social-icon-btn"
            >
              {{ icon }}
            </a>
          </div>
        </div>

        <!-- Links Column 1: Ecosystem -->
        <div class="links-column">
          <h4 class="column-title">Ecosystem</h4>
          <div class="links-list">
            <NuxtLink 
              v-for="link in ecosystemLinks" 
              :key="link.label" 
              :to="link.to" 
              class="footer-link"
            >
              {{ link.label }}
            </NuxtLink>
          </div>
        </div>

        <!-- Links Column 2: Corporate -->
        <div class="links-column">
          <h4 class="column-title">Corporate</h4>
          <div class="links-list">
            <NuxtLink 
              v-for="link in corporateLinks" 
              :key="link.label" 
              :to="link.to" 
              class="footer-link"
            >
              {{ link.label }}
            </NuxtLink>
          </div>
        </div>

        <!-- Links Column 3: Platform -->
        <div class="links-column">
          <h4 class="column-title">Platform</h4>
          <div class="links-list">
            <a 
              href="https://os.zettasyn.com" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="footer-link"
            >
              Team Portal ↗
            </a>
            <NuxtLink 
              v-for="link in platformLinks" 
              :key="link.label" 
              :to="link.to" 
              class="footer-link"
            >
              {{ link.label }}
            </NuxtLink>
          </div>
        </div>
      </div>

      <!-- Bottom copyright -->
      <div class="footer-bottom">
        <span class="copyright-text">
          © {{ currentYear }} Zettasyn Pvt Ltd. All rights reserved.
        </span>
        <span class="designer-text">
          Infrastructure engineered by Zettasyn.
        </span>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { computed } from 'vue'

const currentYear = computed(() => new Date().getFullYear())
const socialIcons = ['𝕏', '💼', '📸', '🎥']

const ecosystemLinks = [
  { label: 'Media Ventures', to: { path: '/', hash: '#firms' } },
  { label: 'Esports Ventures', to: { path: '/', hash: '#firms' } },
  { label: 'Studios Ventures', to: { path: '/', hash: '#firms' } },
  { label: 'Infrastructure', to: { path: '/', hash: '#firms' } }
]

const corporateLinks = [
  { label: 'About Us', to: { path: '/', hash: '#objectives' } },
  { label: 'Careers', to: { path: '/', hash: '#careers' } },
  { label: 'Leadership', to: { path: '/', hash: '#team' } },
  { label: 'Contact', to: { path: '/', hash: '#contact' } }
]

const platformLinks = [
  { label: 'Terms of Service', to: '/terms' },
  { label: 'Privacy Policy', to: '/privacy' }
]
</script>

<style scoped>
.footer-section {
  background: #09090b;
  border-top: 1px solid #18181b;
  padding: 80px 24px 40px 24px;
  position: relative;
  z-index: 5;
}

.footer-container {
  max-width: 1200px;
  margin: 0 auto;
}

.footer-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 48px;
  margin-bottom: 64px;
}

.brand-column {
  grid-column: span 2;
  max-width: 320px;
}

@media (max-width: 768px) {
  .brand-column {
    grid-column: span 1;
  }
}

.brand-name {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 22px;
  color: #ffffff;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  display: block;
  margin-bottom: 16px;
}

.brand-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 14px;
  line-height: 1.6;
  color: #71717a;
  margin-bottom: 24px;
}

.social-icons {
  display: flex;
  gap: 16px;
}

.social-icon-btn {
  width: 36px;
  height: 36px;
  border-radius: 8px;
  background: rgba(24, 24, 27, 0.6);
  border: 1px solid #1f1f23;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #a1a1aa;
  text-decoration: none;
  font-size: 14px;
  transition: all 0.2s ease;
}

.social-icon-btn:hover {
  border-color: #52525b;
  color: #ffffff;
}

.column-title {
  font-size: 12px;
  font-weight: 600;
  color: #ffffff;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  margin-bottom: 20px;
}

.links-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.footer-link {
  font-family: 'Outfit', sans-serif;
  font-size: 14px;
  color: #71717a;
  text-decoration: none;
  transition: color 0.2s ease;
}

.footer-link:hover {
  color: #ffffff;
}

.footer-bottom {
  border-top: 1px solid #1f1f23;
  padding-top: 32px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 16px;
}

.copyright-text {
  font-family: 'Outfit', sans-serif;
  font-size: 13px;
  color: #71717a;
}

.designer-text {
  font-family: 'Outfit', sans-serif;
  font-size: 12px;
  color: #3f3f46;
}
</style>
