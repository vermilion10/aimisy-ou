<template>
  <div class="seal-layer" aria-hidden="true">
    <span v-for="n in 24" :key="n" class="seal-float" :style="sealStyle(n)">🦭</span>
  </div>
</template>

<script setup>
function sealStyle(n) {
  const x = ((n * 137.508) % 100)
  const y = ((n * 79.381) % 100)
  const delay = (n * 0.5) % 10
  const size = 0.6 + (n % 3) * 0.25
  return {
    left: x + '%',
    top: y + '%',
    fontSize: size + 'rem',
    animationDelay: delay + 's',
  }
}
</script>

<style scoped>
.seal-layer {
  position: fixed; inset: 0;
  pointer-events: none; z-index: 0;
  overflow: hidden;
}
.seal-float {
  position: absolute;
  animation: sealFloat 14s ease-in-out infinite alternate;
  user-select: none;
  opacity: 0.05;
}
[data-theme="dark"] .seal-float { opacity: 0.04; }

@keyframes sealFloat {
  0%   { transform: translateY(0) rotate(-4deg) scale(1); }
  50%  { transform: translateY(-20px) rotate(4deg) scale(1.06); }
  100% { transform: translateY(6px) rotate(-2deg) scale(0.96); }
}
</style>
