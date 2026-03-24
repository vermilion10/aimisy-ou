<script setup>
const seals = [
    { x: "5%", y: "15%", size: 32, delay: 0 },
    { x: "85%", y: "25%", size: 28, delay: 1.2 },
    { x: "15%", y: "65%", size: 24, delay: 0.6 },
    { x: "75%", y: "70%", size: 36, delay: 1.8 },
    { x: "45%", y: "85%", size: 20, delay: 2.4 },
    { x: "90%", y: "50%", size: 22, delay: 0.3 },
    { x: "55%", y: "10%", size: 26, delay: 1.5 },
];
</script>

<template>
  <div class="floating-seals">
    <div 
      v-for="(seal, i) in seals" 
      :key="i"
      class="seal-item animate-float"
      :style="{ 
        left: seal.x, 
        top: seal.y, 
        fontSize: seal.size + 'px',
        animationDelay: seal.delay + 's'
      }"
    >
      🦭
    </div>
  </div>
</template>

<style scoped>
.floating-seals {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
}

.seal-item {
  position: absolute;
  user-select: none;
  opacity: 0.15;
  transition: opacity 0.5s ease;
}

[data-theme="dark"] .seal-item {
  opacity: 0.25;
}
</style>
