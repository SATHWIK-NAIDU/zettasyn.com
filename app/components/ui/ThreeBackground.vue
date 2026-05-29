<template>
  <canvas ref="canvas" class="webgl-canvas"></canvas>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import * as THREE from 'three'

const canvas = ref(null)
let renderer, scene, camera, particles, animationId
let uniforms
let mechanicalGroup

// Scroll interaction variables
let targetScrollY = 0
let currentScrollY = 0

onMounted(() => {
  initThree()
  animate()
  window.addEventListener('resize', onWindowResize)
  window.addEventListener('scroll', onScroll, { passive: true })
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', onWindowResize)
  window.removeEventListener('scroll', onScroll)
  cancelAnimationFrame(animationId)
  if (renderer) {
    renderer.dispose()
  }
})

function onScroll() {
  targetScrollY = window.scrollY
}

function createGlowTexture() {
  const canvas = document.createElement('canvas')
  canvas.width = 32
  canvas.height = 32
  const ctx = canvas.getContext('2d')
  
  const gradient = ctx.createRadialGradient(16, 16, 0, 16, 16, 16)
  gradient.addColorStop(0, 'rgba(255,255,255,1)')
  gradient.addColorStop(0.1, 'rgba(255,255,255,0.8)')
  gradient.addColorStop(0.4, 'rgba(255,255,255,0.2)')
  gradient.addColorStop(1, 'rgba(0,0,0,0)')
  
  ctx.fillStyle = gradient
  ctx.fillRect(0, 0, 32, 32)
  
  return new THREE.CanvasTexture(canvas)
}

function initThree() {
  scene = new THREE.Scene()
  scene.fog = new THREE.FogExp2(0x050505, 0.0008)

  camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 1, 2000)
  camera.position.z = 1000

  // Particle System
  const geometry = new THREE.BufferGeometry()
  const vertices = []
  const colors = []
  const sizes = []
  const phases = []
  
  // Space colors + ZettaSyn accents
  const color1 = new THREE.Color(0xffffff) // White star
  const color2 = new THREE.Color(0xa1c4fd) // Light blue star
  const color3 = new THREE.Color(0x00f0ff) // Cyan (ZettaSyn)
  const color4 = new THREE.Color(0x9d00ff) // Purple (ZettaSyn)

  for (let i = 0; i < 4000; i++) {
    // Spread them far out to simulate depth of space
    const x = Math.random() * 3000 - 1500
    const y = Math.random() * 3000 - 1500
    const z = Math.random() * 3000 - 1500
    vertices.push(x, y, z)

    const r = Math.random()
    let col = color1
    if (r > 0.6) col = color2
    if (r > 0.85) col = color3
    if (r > 0.95) col = color4
    
    colors.push(col.r, col.g, col.b)
    
    // Randomize base size and blinking phase for very small stars
    sizes.push(Math.random() * 5 + 2)
    phases.push(Math.random() * Math.PI * 2)
  }

  geometry.setAttribute('position', new THREE.Float32BufferAttribute(vertices, 3))
  geometry.setAttribute('color', new THREE.Float32BufferAttribute(colors, 3))
  geometry.setAttribute('size', new THREE.Float32BufferAttribute(sizes, 1))
  geometry.setAttribute('phase', new THREE.Float32BufferAttribute(phases, 1))

  uniforms = {
    time: { value: 0 },
    pointTexture: { value: createGlowTexture() }
  }

  const material = new THREE.ShaderMaterial({
    uniforms: uniforms,
    vertexShader: `
      attribute float size;
      attribute float phase;
      varying vec3 vColor;
      varying float vPhase;
      void main() {
        vColor = color;
        vPhase = phase;
        vec4 mvPosition = modelViewMatrix * vec4(position, 1.0);
        gl_PointSize = size * (300.0 / -mvPosition.z);
        gl_Position = projectionMatrix * mvPosition;
      }
    `,
    fragmentShader: `
      uniform float time;
      uniform sampler2D pointTexture;
      varying vec3 vColor;
      varying float vPhase;
      void main() {
        // Create a twinkling effect using sine wave based on time and individual phase
        float alpha = 0.3 + 0.7 * sin(time * 2.0 + vPhase);
        vec4 texColor = texture2D(pointTexture, gl_PointCoord);
        gl_FragColor = vec4(vColor, alpha * texColor.a);
      }
    `,
    blending: THREE.AdditiveBlending,
    depthWrite: false,
    transparent: true,
    vertexColors: true
  })

  particles = new THREE.Points(geometry, material)
  scene.add(particles)

  renderer = new THREE.WebGLRenderer({ canvas: canvas.value, alpha: true, antialias: true })
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  renderer.setSize(window.innerWidth, window.innerHeight)

  // Mechanical Geometry System
  mechanicalGroup = new THREE.Group()
  scene.add(mechanicalGroup)

  const geometries = [
    new THREE.IcosahedronGeometry(15, 0),
    new THREE.OctahedronGeometry(20, 0),
    new THREE.TetrahedronGeometry(25, 0)
  ]

  const wireframeMaterial = new THREE.LineBasicMaterial({
    color: 0x00f0ff,
    transparent: true,
    opacity: 0.15,
    blending: THREE.AdditiveBlending
  })

  const solidMaterial = new THREE.MeshStandardMaterial({
    color: 0x0a0a0f,
    metalness: 0.9,
    roughness: 0.2,
    envMapIntensity: 1.0,
    transparent: true,
    opacity: 0.8
  })

  // Add lights for metallic shading
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
  scene.add(ambientLight)
  
  const pointLight1 = new THREE.PointLight(0x00f0ff, 500, 1000)
  pointLight1.position.set(200, 200, 200)
  scene.add(pointLight1)
  
  const pointLight2 = new THREE.PointLight(0x9d00ff, 500, 1000)
  pointLight2.position.set(-200, -200, 200)
  scene.add(pointLight2)

  // Create 40 floating mechanical shards
  for (let i = 0; i < 40; i++) {
    const geo = geometries[Math.floor(Math.random() * geometries.length)]
    
    // Core solid shape
    const mesh = new THREE.Mesh(geo, solidMaterial)
    
    // Wireframe outline
    const edges = new THREE.EdgesGeometry(geo)
    const line = new THREE.LineSegments(edges, wireframeMaterial)
    mesh.add(line)

    // Random placement
    mesh.position.x = (Math.random() - 0.5) * 800
    mesh.position.y = (Math.random() - 0.5) * 800
    mesh.position.z = (Math.random() - 0.5) * 800 - 200

    mesh.rotation.x = Math.random() * Math.PI
    mesh.rotation.y = Math.random() * Math.PI

    // Custom animation properties
    mesh.userData = {
      rotSpeedX: (Math.random() - 0.5) * 0.005,
      rotSpeedY: (Math.random() - 0.5) * 0.005,
      floatSpeed: (Math.random() - 0.5) * 0.5,
      baseY: mesh.position.y,
      phase: Math.random() * Math.PI * 2
    }

    mechanicalGroup.add(mesh)
  }
}

function onWindowResize() {
  camera.aspect = window.innerWidth / window.innerHeight
  camera.updateProjectionMatrix()
  renderer.setSize(window.innerWidth, window.innerHeight)
}

function animate() {
  animationId = requestAnimationFrame(animate)

  // Cinematic scroll interpolation (Lerp)
  currentScrollY += (targetScrollY - currentScrollY) * 0.05
  
  // Parallax camera based on scroll
  camera.position.y = -currentScrollY * 0.1

  if (particles) {
    // Very slow natural rotation
    particles.rotation.y += 0.0002
    particles.rotation.x += 0.0001
  }

  if (mechanicalGroup) {
    mechanicalGroup.rotation.y = currentScrollY * 0.0002
    
    // Animate each shard
    mechanicalGroup.children.forEach(child => {
      child.rotation.x += child.userData.rotSpeedX
      child.rotation.y += child.userData.rotSpeedY
      // Gentle floating
      child.position.y = child.userData.baseY + Math.sin(uniforms.time.value * 2.0 + child.userData.phase) * 10
    })
  }

  if (uniforms) {
    // Update time for twinkling shader
    uniforms.time.value += 0.02
  }

  renderer.render(scene, camera)
}
</script>

<style scoped>
.webgl-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: -1;
  pointer-events: none;
}
</style>
