<template>
  <section id="contact" class="contact-section">
    <div class="contact-container">
      <!-- Section Header -->
      <div class="section-header">
        <span class="section-label block-label">Get In Touch</span>
        <h2 class="section-title">Connect With Our Leadership</h2>
        <p class="section-desc">
          Whether seeking strategic synergy, licensing, or corporate partnership, reach out directly to start the conversation.
        </p>
      </div>

      <!-- Dual Column Layout -->
      <div class="contact-layout">
        <!-- Left Column: Direct Info -->
        <div class="info-column">
          <div>
            <h3 class="info-title">Inquiries & Partnerships</h3>
            <p class="info-desc">
              Our investment and operations leadership actively monitors communications for strategic synergy matching.
            </p>
          </div>

          <!-- Contacts Grid -->
          <div class="contacts-list">
            <div 
              v-for="(item, idx) in contacts" 
              :key="idx" 
              class="contact-item"
            >
              <div class="contact-icon">
                <component :is="item.icon" />
              </div>
              <div>
                <div class="contact-label">{{ item.title }}</div>
                <div class="contact-detail">{{ item.detail }}</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Right Column: Glassmorphic Interactive Form -->
        <div class="form-column">
          <div v-if="submitted" class="success-box">
            <div class="success-icon">✓</div>
            <h4 class="success-title">Inquiry Sent Successfully</h4>
            <p class="success-desc">
              Thank you for reaching out. A partner officer will respond within 24 hours.
            </p>
          </div>
          
          <form v-else @submit.prevent="handleSubmit" class="contact-form">
            <div class="form-row">
              <div class="form-group flex-1">
                <label class="input-label">Your Name</label>
                <input
                  v-model="formState.name"
                  type="text"
                  required
                  placeholder="e.g. Rahul Sharma"
                  class="form-input"
                />
              </div>
              <div class="form-group flex-1">
                <label class="input-label">Company</label>
                <input
                  v-model="formState.company"
                  type="text"
                  placeholder="e.g. Media Venture"
                  class="form-input"
                />
              </div>
            </div>

            <div class="form-group">
              <label class="input-label">Email Address</label>
              <input
                v-model="formState.email"
                type="email"
                required
                placeholder="name@company.com"
                class="form-input"
              />
            </div>

            <div class="form-group">
              <label class="input-label">Inquiry Message</label>
              <textarea
                v-model="formState.message"
                required
                rows="4"
                placeholder="Detail the scope of synergy or partnership desired..."
                class="form-textarea"
              />
            </div>

            <button type="submit" class="gradient-btn submit-btn">
              Send Partnership Message →
            </button>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, h } from 'vue'

const formState = ref({
  name: '',
  email: '',
  company: '',
  message: '',
})
const submitted = ref(false)

const handleSubmit = () => {
  submitted.value = true
  setTimeout(() => {
    submitted.value = false
    formState.value = { name: '', email: '', company: '', message: '' }
  }, 3000)
}

const contacts = [
  {
    title: 'Corporate Partnerships',
    detail: 'partners@zettasyn.com',
    icon: {
      render: () => h('svg', { width: '20', height: '20', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '2', strokeLinecap: 'round', strokeLinejoin: 'round', style: { color: 'var(--grad-from)' } }, [
        h('path', { d: 'M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2' }),
        h('circle', { cx: '9', cy: '7', r: '4' }),
        h('path', { d: 'M22 21v-2a4 4 0 0 0-3-3.87' }),
        h('path', { d: 'M16 3.13a4 4 0 0 1 0 7.75' })
      ])
    }
  },
  {
    title: 'Press & Media Relations',
    detail: 'media@zettasyn.com',
    icon: {
      render: () => h('svg', { width: '20', height: '20', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '2', strokeLinecap: 'round', strokeLinejoin: 'round', style: { color: 'var(--grad-to)' } }, [
        h('path', { d: 'M4 22V4c0-.5.2-1 .6-1.4C5 2.2 5.5 2 6 2h12c.5 0 1 .2 1.4.6.4.4.6.9.6 1.4v18l-4-2-4 2-4-2-4 2z' }),
        h('path', { d: 'M12 2v16' }),
        h('path', { d: 'M8 6h8' }),
        h('path', { d: 'M8 10h8' })
      ])
    }
  },
  {
    title: 'Headquarters Location',
    detail: 'New Delhi, India',
    icon: {
      render: () => h('svg', { width: '20', height: '20', viewBox: '0 0 24 24', fill: 'none', stroke: 'currentColor', strokeWidth: '2', strokeLinecap: 'round', strokeLinejoin: 'round', style: { color: '#3b82f6' } }, [
        h('path', { d: 'M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z' }),
        h('circle', { cx: '12', cy: '10', r: '3' })
      ])
    }
  }
]
</script>

<style scoped>
.contact-section {
  padding: 100px 24px;
  background: #09090b;
  border-top: 1px solid #18181b;
  position: relative;
  z-index: 5;
}

.contact-container {
  max-width: 1100px;
  margin: 0 auto;
}

.section-header {
  text-align: center;
  margin-bottom: 70px;
}

.block-label {
  display: block;
  margin-bottom: 12px;
}

.section-title {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: clamp(28px, 5vw, 42px);
  color: #ffffff;
  letter-spacing: -0.01em;
  margin-bottom: 16px;
}

.section-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 16px;
  font-weight: 300;
  color: #71717a;
  max-width: 600px;
  margin: 0 auto;
}

.contact-layout {
  display: flex;
  flex-wrap: wrap;
  gap: 48px;
  justify-content: space-between;
  align-items: stretch;
}

.info-column {
  flex: 1 1 380px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 40px;
}

.info-title {
  font-family: 'Outfit', sans-serif;
  font-weight: 600;
  font-size: 24px;
  color: #ffffff;
  margin-bottom: 16px;
}

.info-desc {
  font-family: 'Outfit', sans-serif;
  font-size: 15px;
  line-height: 1.6;
  color: #a1a1aa;
  margin-bottom: 28px;
}

.contacts-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
  border-radius: 12px;
  background: rgba(24, 24, 27, 0.4);
  border: 1px solid #1f1f23;
}

.contact-icon {
  width: 40px;
  height: 40px;
  border-radius: 8px;
  background: rgba(39, 39, 42, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
}

.contact-label {
  font-size: 11px;
  color: #71717a;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.contact-detail {
  font-size: 15px;
  color: #ffffff;
  font-weight: 500;
}

.form-column {
  flex: 1 1 480px;
  background: linear-gradient(135deg, rgba(24,24,27,0.3) 0%, rgba(9,9,11,0.5) 100%);
  border: 1px solid #27272a;
  border-radius: 20px;
  padding: 40px;
  box-shadow: 0 20px 45px rgba(0,0,0,0.4);
  backdrop-filter: blur(12px);
}

.success-box {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 40px 0;
}

.success-icon {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  background: rgba(34, 197, 94, 0.1);
  border: 1px solid #22c55e;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #22c55e;
  font-size: 28px;
  margin-bottom: 24px;
}

.success-title {
  font-family: 'Outfit', sans-serif;
  color: #ffffff;
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 8px;
}

.success-desc {
  font-family: 'Outfit', sans-serif;
  color: #a1a1aa;
  font-size: 14px;
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-row {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
}

.flex-1 {
  flex: 1 1 200px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.input-label {
  font-size: 12px;
  font-weight: 500;
  color: #a1a1aa;
  text-transform: uppercase;
  letter-spacing: 0.04em;
}

.form-input {
  padding: 12px 16px;
  background: rgba(9, 9, 11, 0.6);
  border: 1px solid #27272a;
  border-radius: 8px;
  color: #ffffff;
  font-size: 14px;
  font-family: 'Outfit', sans-serif;
  outline: none;
}

.form-textarea {
  padding: 12px 16px;
  background: rgba(9, 9, 11, 0.6);
  border: 1px solid #27272a;
  border-radius: 8px;
  color: #ffffff;
  font-size: 14px;
  font-family: 'Outfit', sans-serif;
  outline: none;
  resize: none;
}

.submit-btn {
  padding: 14px 20px;
  border-radius: 8px;
  font-size: 14px;
  margin-top: 8px;
}
</style>
