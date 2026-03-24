<template>
  <canvas ref="canvas" class="particle-canvas" aria-hidden="true"></canvas>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref(null)
let ctx, animId
const particles = []

function resize() {
  canvas.value.width = window.innerWidth
  canvas.value.height = window.innerHeight
}

function init() {
  resize()
  for (let i = 0; i < 60; i++) {
    particles.push({
      x: Math.random() * canvas.value.width,
      y: Math.random() * canvas.value.height,
      r: Math.random() * 2.5 + 0.5,
      dx: (Math.random() - 0.5) * 0.4,
      dy: -(Math.random() * 0.5 + 0.2),
      alpha: Math.random() * 0.6 + 0.2,
      hue: 320 + Math.random() * 40,
    })
  }
}

function draw() {
  ctx.clearRect(0, 0, canvas.value.width, canvas.value.height)
  particles.forEach(p => {
    ctx.beginPath()
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2)
    ctx.fillStyle = `hsla(${p.hue}, 100%, 70%, ${p.alpha})`
    ctx.fill()
    p.x += p.dx
    p.y += p.dy
    p.alpha -= 0.001
    if (p.y < -10 || p.alpha <= 0) {
      p.x = Math.random() * canvas.value.width
      p.y = canvas.value.height + 10
      p.alpha = Math.random() * 0.6 + 0.2
    }
  })
  animId = requestAnimationFrame(draw)
}

onMounted(() => {
  ctx = canvas.value.getContext('2d')
  init()
  draw()
  window.addEventListener('resize', resize)
})

onUnmounted(() => {
  cancelAnimationFrame(animId)
  window.removeEventListener('resize', resize)
})
</script>

<style scoped>
.particle-canvas {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 1;
}
</style>
