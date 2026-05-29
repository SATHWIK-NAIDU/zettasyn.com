<template>
  <section id="contact" class="contact-section">
    <div class="contact-trigger-wrapper" ref="triggerWrapper">
      <button class="command-center-btn" @click="openCommandCenter">
        <span class="btn-text">Initialize Contact</span>
        <div class="btn-scanline"></div>
      </button>
    </div>

    <!-- Immersive Overlay -->
    <Teleport to="body">
      <div 
        class="command-center-overlay" 
        :class="{ 'is-active': isOpen }"
        ref="overlay"
      >
        <!-- Background Elements -->
        <div class="overlay-bg"></div>
        <div class="volumetric-light"></div>
        
        <!-- Animated Wireframe/Grid -->
        <div class="wireframe-grid">
          <div class="grid-horizontal"></div>
          <div class="grid-vertical"></div>
        </div>

        <button class="close-btn" @click="closeCommandCenter">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M18 6L6 18M6 6l12 12" />
          </svg>
        </button>

        <div class="holographic-form-container">
          <div class="form-header">
            <h2>Establish Connection</h2>
            <div class="status-indicator">
              <span class="dot"></span>
              <span>Secure Channel</span>
            </div>
          </div>

          <Transition name="fade" mode="out-in">
            <div v-if="isSubmitted" class="success-message">
              <div class="success-icon">
                <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M20 6L9 17l-5-5" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </div>
              <h3>Transmission Successful</h3>
              <p>Our team will be in touch within 6 - 12 hours (except holidays) from your mail received.</p>
            </div>

            <form v-else class="holo-form" @submit.prevent="submitForm">
            <div class="input-group">
              <input type="text" id="name" v-model="form.name" required class="holo-input" placeholder=" " />
              <label for="name" class="holo-label">Designation (Name)</label>
              <div class="input-border"></div>
              <div class="input-glow"></div>
            </div>

            <div class="input-group">
              <input type="email" id="email" v-model="form.email" required class="holo-input" placeholder=" " />
              <label for="email" class="holo-label">Network ID (Email)</label>
              <div class="input-border"></div>
              <div class="input-glow"></div>
            </div>

            <div class="input-group">
              <input type="tel" id="phone" v-model="form.phone" required class="holo-input" placeholder=" " />
              <label for="phone" class="holo-label">Comlink Frequency (Mobile)</label>
              <div class="input-border"></div>
              <div class="input-glow"></div>
            </div>

            <div class="input-group">
              <textarea id="message" v-model="form.message" required class="holo-input holo-textarea" placeholder=" " rows="4"></textarea>
              <label for="message" class="holo-label">Encrypted Transmission (Message)</label>
              <div class="input-border"></div>
              <div class="input-glow"></div>
            </div>

            <button type="submit" class="submit-btn" ref="submitBtn" @mousemove="onSubmitMove" @mouseleave="onSubmitLeave">
              <span class="btn-text">Transmit Data</span>
              <div class="submit-glow"></div>
            </button>
            </form>
          </Transition>
        </div>
      </div>
    </Teleport>
  </section>
</template>

<script setup>
import { ref, reactive } from 'vue'
import gsap from 'gsap'

const isOpen = ref(false)
const isSubmitted = ref(false)
const overlay = ref(null)

const form = reactive({
  name: '',
  email: '',
  phone: '',
  message: ''
})

function openCommandCenter() {
  isOpen.value = true
  isSubmitted.value = false
  form.name = ''
  form.email = ''
  form.phone = ''
  form.message = ''
  document.body.style.overflow = 'hidden'

  // Cinematic opening sequence
  const tl = gsap.timeline()
  
  tl.set('.command-center-overlay', { display: 'flex' })
  tl.fromTo('.overlay-bg', 
    { opacity: 0 },
    { opacity: 1, duration: 1, ease: 'power2.inOut' }
  )
  
  tl.fromTo('.wireframe-grid',
    { opacity: 0, scale: 1.5, rotationX: 60 },
    { opacity: 0.3, scale: 1, rotationX: 45, duration: 1.5, ease: 'power3.out' },
    "-=0.5"
  )
  
  tl.fromTo('.holographic-form-container',
    { opacity: 0, y: 100, scale: 0.9, filter: 'blur(20px)' },
    { opacity: 1, y: 0, scale: 1, filter: 'blur(0px)', duration: 1, ease: 'back.out(1.2)' },
    "-=1"
  )
  
  tl.fromTo('.input-group',
    { opacity: 0, x: -50 },
    { opacity: 1, x: 0, duration: 0.6, stagger: 0.1, ease: 'power2.out' },
    "-=0.5"
  )
}

function closeCommandCenter() {
  const tl = gsap.timeline({
    onComplete: () => {
      isOpen.value = false
      document.body.style.overflow = ''
      gsap.set('.command-center-overlay', { display: 'none' })
    }
  })
  
  tl.to('.holographic-form-container', {
    opacity: 0,
    y: 50,
    scale: 0.95,
    filter: 'blur(10px)',
    duration: 0.5,
    ease: 'power2.in'
  })
  
  tl.to('.wireframe-grid', {
    opacity: 0,
    scale: 1.2,
    duration: 0.5,
    ease: 'power2.in'
  }, "-=0.3")
  
  tl.to('.overlay-bg', {
    opacity: 0,
    duration: 0.5,
    ease: 'power2.inOut'
  }, "-=0.2")
}

function submitForm() {
  // Simulate transmission animation
  gsap.to('.submit-btn', {
    scale: 0.95,
    duration: 0.1,
    yoyo: true,
    repeat: 1
  })
  
  gsap.to('.holographic-form-container', {
    filter: 'hue-rotate(90deg) brightness(1.5)',
    duration: 0.5,
    yoyo: true,
    repeat: 1,
    onComplete: () => {
      const to = 'contact@zettasyn.com'
      const subject = encodeURIComponent(`Inquiry from ${form.name}`)
      const bodyText = `Name: ${form.name}\nEmail: ${form.email}\nMobile: ${form.phone}\n\nMessage:\n${form.message}`
      const body = encodeURIComponent(bodyText)

      // Simple mobile/tablet detection
      const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) || window.innerWidth < 1024;

      if (isMobile) {
        // Trigger native mail app (e.g. Gmail/Mail) on mobile devices
        window.location.href = `mailto:${to}?subject=${subject}&body=${body}`
      } else {
        // Open Gmail compose window in a new tab on desktop
        const gmailUrl = `https://mail.google.com/mail/?view=cm&fs=1&to=${to}&su=${subject}&body=${body}`
        window.open(gmailUrl, '_blank')
      }

      // Show success message
      isSubmitted.value = true

      // Close automatically after 5 seconds
      setTimeout(() => {
        if (isOpen.value) closeCommandCenter()
      }, 5000)
    }
  })
}

function onSubmitMove(e) {
  const btn = e.currentTarget
  const rect = btn.getBoundingClientRect()
  const x = e.clientX - rect.left - rect.width / 2
  const y = e.clientY - rect.top - rect.height / 2
  
  gsap.to(btn, {
    x: x * 0.1,
    y: y * 0.1,
    duration: 0.3,
    ease: 'power2.out'
  })
}

function onSubmitLeave(e) {
  const btn = e.currentTarget
  gsap.to(btn, {
    x: 0,
    y: 0,
    duration: 0.5,
    ease: 'elastic.out(1, 0.3)'
  })
}
</script>

<style scoped>
.contact-section {
  padding: 120px 24px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 50vh;
}

.command-center-btn {
  position: relative;
  background: rgba(17, 17, 21, 0.8);
  border: 1px solid var(--grad-to);
  color: #fff;
  font-family: var(--font-heading);
  font-size: 24px;
  font-weight: 600;
  padding: 24px 64px;
  cursor: pointer;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(0, 240, 255, 0.2);
  transition: box-shadow 0.3s, transform 0.3s;
}

.command-center-btn:hover {
  box-shadow: 0 0 40px rgba(0, 240, 255, 0.4), inset 0 0 20px rgba(0, 240, 255, 0.2);
  transform: scale(1.02);
}

.btn-scanline {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: var(--grad-to);
  opacity: 0.5;
  box-shadow: 0 0 10px var(--grad-to);
  animation: scan 2s linear infinite;
}

@keyframes scan {
  0% { top: -10%; }
  100% { top: 110%; }
}

/* Command Center Overlay */
.command-center-overlay {
  position: fixed;
  inset: 0;
  z-index: 10000;
  display: none;
  justify-content: center;
  align-items: center;
  perspective: 1000px;
}

.overlay-bg {
  position: absolute;
  inset: 0;
  background: rgba(5, 5, 5, 0.95);
  backdrop-filter: blur(20px);
  z-index: 0;
}

.volumetric-light {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at center 30%, rgba(28, 82, 246, 0.15) 0%, transparent 60%);
  z-index: 1;
  pointer-events: none;
}

.wireframe-grid {
  position: absolute;
  inset: -50%;
  background-image: 
    linear-gradient(rgba(0, 240, 255, 0.1) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0, 240, 255, 0.1) 1px, transparent 1px);
  background-size: 50px 50px;
  z-index: 1;
  transform-origin: center center;
  pointer-events: none;
}

.close-btn {
  position: absolute;
  top: 40px;
  right: 40px;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: #fff;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: var(--grad-to);
  color: var(--grad-to);
  transform: rotate(90deg);
}

.holographic-form-container {
  position: relative;
  z-index: 5;
  width: 100%;
  max-width: 600px;
  background: rgba(10, 10, 12, 0.7);
  border: 1px solid rgba(0, 240, 255, 0.3);
  border-radius: 16px;
  padding: 48px;
  box-shadow: 
    0 30px 60px rgba(0, 0, 0, 0.8),
    inset 0 0 40px rgba(0, 240, 255, 0.05);
  transform-style: preserve-3d;
}

.form-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 40px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  padding-bottom: 16px;
}

.form-header h2 {
  font-family: var(--font-heading);
  font-size: 24px;
  color: #fff;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.status-indicator {
  display: flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-body);
  font-size: 12px;
  color: var(--grad-to);
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.status-indicator .dot {
  width: 6px;
  height: 6px;
  background: var(--grad-to);
  border-radius: 50%;
  box-shadow: 0 0 8px var(--grad-to);
  animation: pulse-dot 1.5s infinite;
}

@keyframes pulse-dot {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.4; }
}

.holo-form {
  display: flex;
  flex-direction: column;
  gap: 32px;
}

.input-group {
  position: relative;
}

.holo-input {
  width: 100%;
  background: rgba(255, 255, 255, 0.03);
  border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  padding: 16px 16px 12px 16px;
  color: #fff;
  font-family: var(--font-body);
  font-size: 16px;
  transition: background 0.3s;
  outline: none;
}

.holo-textarea {
  resize: vertical;
  min-height: 100px;
}

.holo-label {
  position: absolute;
  top: 16px;
  left: 16px;
  font-family: var(--font-body);
  font-size: 14px;
  color: var(--text-secondary);
  pointer-events: none;
  transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.input-border {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0%;
  height: 2px;
  background: var(--grad-to);
  transition: width 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.input-glow {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at 50% 50%, rgba(0, 240, 255, 0.1), transparent 70%);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s;
}

.holo-input:focus,
.holo-input:not(:placeholder-shown) {
  background: rgba(0, 240, 255, 0.05);
}

.holo-input:focus ~ .holo-label,
.holo-input:not(:placeholder-shown) ~ .holo-label {
  top: -10px;
  font-size: 11px;
  color: var(--grad-to);
}

.holo-input:focus ~ .input-border {
  width: 100%;
}

.holo-input:focus ~ .input-glow {
  opacity: 1;
}

.submit-btn {
  margin-top: 16px;
  background: linear-gradient(135deg, rgba(28, 82, 246, 0.2), rgba(0, 240, 255, 0.2));
  border: 1px solid rgba(0, 240, 255, 0.5);
  color: #fff;
  padding: 16px;
  font-family: var(--font-heading);
  font-size: 16px;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  cursor: pointer;
  border-radius: 4px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s;
}

.submit-btn:hover {
  background: linear-gradient(135deg, rgba(28, 82, 246, 0.4), rgba(0, 240, 255, 0.4));
  box-shadow: 0 0 20px rgba(0, 240, 255, 0.3);
}

.submit-glow {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(circle, rgba(255,255,255,0.2) 0%, transparent 50%);
  opacity: 0;
  transform: translate(0, 0);
  transition: opacity 0.3s;
  pointer-events: none;
}

.submit-btn:hover .submit-glow {
  opacity: 1;
}

@media (max-width: 768px) {
  .holographic-form-container {
    padding: 24px;
    margin: 16px;
  }
  
  .form-header h2 {
    font-size: 18px;
  }
}

/* Success Message Styles */
.success-message {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 40px 0;
}

.success-icon {
  color: var(--grad-to);
  margin-bottom: 24px;
  animation: float-success 3s ease-in-out infinite;
  filter: drop-shadow(0 0 10px rgba(0, 240, 255, 0.5));
}

@keyframes float-success {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.success-message h3 {
  font-family: var(--font-heading);
  font-size: 24px;
  color: #fff;
  margin-bottom: 16px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.success-message p {
  font-family: var(--font-body);
  font-size: 16px;
  color: var(--text-secondary);
  line-height: 1.6;
  max-width: 80%;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
