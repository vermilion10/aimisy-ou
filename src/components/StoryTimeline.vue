<template>
  <section class="timeline-section" id="timeline" ref="sectionRef">
    <div class="header text-center reveal">
      <span class="tag"> </span>
      <h2 class="section-title">Key Moments</h2>
      <div class="header-seals">
        <span v-for="i in 3" :key="i" class="animate-float" :style="{ animationDelay: i * 0.3 + 's' }">
          {{ ['🦭', '💗', '✨'][i-1] }}
        </span>
      </div>
    </div>

    <div class="timeline-container container">
      <div class="timeline-track"></div>
      <div class="timeline-progress" :style="{ height: progressHeight + '%' }"></div>
      <div class="nodes-wrap">
        <div v-for="(group, gIdx) in groups" :key="group.id" class="timeline-node" :ref="el => nodeRefs[gIdx] = el">
          <div class="node-dot" :class="{ 'node-active': activeNodes[gIdx] }"></div>

          <div class="burst-layout">
            <div 
              v-for="(img, i) in group.images" 
              :key="i"
              class="card-photo"
              :class="{ 'card-visible': activeNodes[gIdx] }"
              :style="getCardStyle(i, gIdx)"
              @click="selectedImg = img.src"
            >
              <div class="polaroid-frame">
                <img :src="img.src" alt="Story CG" loading="lazy" />
              </div>
            </div>
          </div>
        </div>

        <div class="timeline-node" :ref="el => nodeRefs[groups.length] = el">
          <div class="node-dot" :class="{ 'node-active': activeNodes[groups.length] }"></div>
          <div class="burst-layout">
            <div 
              class="card-photo single-card"
              :class="{ 'card-visible': activeNodes[groups.length] }"
              @click="selectedImg = `${baseUrl}themooredboat.jpeg`"
            >
              <div class="polaroid-frame">
                <img :src="`${baseUrl}themooredboat.jpeg`" alt=".. .-- .. .-.. .-.. .--. .-. --- - . -.-. - -.-- --- ..-" loading="lazy" />
                <p class="caption-text">.. .-- .. .-.. .-.. .--. .-. --- - . -.-. - -.-- --- ..-</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <Transition name="fade">
      <div v-if="selectedImg" class="lightbox" @click="selectedImg = null">
        <div class="modal-wrap" @click.stop>
          <div class="modal-box">
            <img :src="selectedImg" alt="Full Image" class="modal-img" />
            <button class="modal-close" @click="selectedImg = null">
              <span class="x-icon">×</span>
            </button>
          </div>
        </div>
      </div>
    </Transition>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

const baseUrl = import.meta.env.BASE_URL
const sectionRef = ref(null)
const nodeRefs = ref([])
const activeNodes = ref({})
const progressHeight = ref(0)
const selectedImg = ref(null)

const allImages = [
  'cg/00.2.jpg', 'cg/01.jpg', 'cg/02.jpg', 'cg/03.2.jpg',
  'cg/14.jpg',   'cg/15.2.jpg', 'cg/16.jpg', 'cg/17.jpg',
  'cg/18.jpg',   'cg/19.2.jpg', 'cg/20.2.jpg', 'cg/21.jpg',
  'cg/22.2.jpg', 'cg/23.2.jpg', 'cg/24.2.jpg', 'cg/25.2.jpg',
  'cg/26.2.jpg', 'cg/27.2.jpg', 'cg/28.2.jpg', 'cg/29.2.jpg',
  'cg/30.2.jpg', 'cg/31.2.jpg', 'cg/32.2.jpg', 'cg/33.2.jpg',
  'cg/34.2.jpg', 'cg/35.2.jpg', 'cg/36.1.jpg', 'cg/37.jpg'
].map(p => `${import.meta.env.BASE_URL}${p}`);

const groups = computed(() => {
  const size = 4
  return Array.from({ length: Math.ceil(allImages.length / size) }, (_, i) => ({
    id: i,
    images: allImages.slice(i * size, i * size + size).map(src => ({ src }))
  }))
})

const BURST_PRESETS = [
  { x: -280, y: -120, rot: -7 },  
  { x: 280,  y: -120, rot: 7 },   
  { x: -280, y: 120,  rot: 8 },   
  { x: 280,  y: 120,  rot: -8 },  
]

function getCardStyle(imgIdx, gIdx) {
  const layout = BURST_PRESETS[imgIdx % BURST_PRESETS.length]
  return {
    '--tx': layout.x + 'px',
    '--ty': layout.y + 'px',
    '--rot': layout.rot + 'deg',
    transitionDelay: (imgIdx * 0.1) + 's'
  }
}

function updateScroll() {
  if (!sectionRef.value) return
  const rect = sectionRef.value.getBoundingClientRect()
  const vh = window.innerHeight
  
  const scrolled = -rect.top + vh/2
  const total = rect.height || 1
  const progress = Math.min(100, Math.max(0, (scrolled / total) * 100))
  progressHeight.value = progress

  nodeRefs.value.forEach((el, idx) => {
    if (!el) return
    const elRect = el.getBoundingClientRect()
    if (elRect.top < vh * 0.8 && elRect.bottom > vh * 0.1) {
      activeNodes.value[idx] = true
    } else {
      activeNodes.value[idx] = false
    }
  })
}

onMounted(() => {
  window.addEventListener('scroll', updateScroll, { passive: true })
  updateScroll()
})
onUnmounted(() => window.removeEventListener('scroll', updateScroll))
</script>

<style scoped>
.timeline-section {
  padding: 8rem 0;
  position: relative;
  overflow: visible;
}

.header { margin-bottom: 6rem; }
.header-seals {
  display: flex;
  justify-content: center;
  gap: 1.2rem;
  margin-top: 1rem;
  font-size: 1.5rem;
}

.timeline-container {
  position: relative;
  max-width: 1000px;
  min-height: 1200px;
}

.timeline-track {
  position: absolute;
  left: 50%;
  top: 0;
  bottom: 160px;
  width: 2px;
  background: var(--border);
  transform: translateX(-50%);
  opacity: 0.3;
}

.timeline-progress {
  position: absolute;
  left: 50%;
  top: 0;
  max-height: calc(100% - 160px);
  width: 2px;
  background: linear-gradient(to bottom, var(--pink-500), var(--pink-300));
  transform: translateX(-50%);
  z-index: 1;
  box-shadow: 0 0 10px var(--pink-300);
  transition: height 0.1s linear;
}

.nodes-wrap {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 14rem;
}

.timeline-node {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 320px;
}

.timeline-node:last-child {
  height: auto;
  padding-bottom: 2rem;
}

.node-dot {
  width: 0.75rem;
  height: 0.75rem;
  background: var(--surface);
  border: 4px solid var(--border);
  border-radius: 50%;
  z-index: 2;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.node-active {
  background: var(--pink-500);
  border-color: var(--pink-200);
  transform: scale(1.6);
  box-shadow: 0 0 15px var(--pink-400);
}

.card-photo {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 260px;
  opacity: 0;
  transform: translate(-50%, -50%) scale(0.6);
  transition: all 0.9s cubic-bezier(0.34, 1.56, 0.64, 1);
  cursor: pointer;
  z-index: 5;
}
.card-visible {
  opacity: 1;
  transform: translate(calc(-50% + var(--tx)), calc(-50% + var(--ty))) scale(1) rotate(var(--rot));
}

.polaroid-frame {
  background: white;
  padding: 8px 8px 32px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  border-radius: 4px;
  border: 1px solid rgba(0,0,0,0.05);
}
.polaroid-frame img {
  width: 100%;
  aspect-ratio: 16/10;
  object-fit: cover;
  border-radius: 2px;
}

.card-photo:hover {
  z-index: 20;
  transform: translate(calc(-50% + var(--tx)), calc(-50% + var(--ty))) scale(1.1) rotate(0);
}

@media (max-width: 768px) {
  .nodes-wrap { gap: 9rem; }
  .card-photo { width: 140px; }
  
  .card-visible {
    transform: translate(calc(-50% + var(--tx) * 0.55), calc(-50% + var(--ty) * 0.7)) scale(1) rotate(var(--rot));
  }
  .card-photo:hover {
    transform: translate(calc(-50% + var(--tx) * 0.55), calc(-50% + var(--ty) * 0.7)) scale(1.05) rotate(0);
  }
}

.single-card {
  --tx: 0px !important;
  --ty: 0px !important;
  --rot: 0deg !important;
  width: 320px;
}
@media (max-width: 768px) {
  .single-card { width: 200px; }
}
.caption-text {
  text-align: center;
  font-weight: 700;
  color: var(--pink-500);
  margin-top: 1rem;
  font-family: 'Playfair Display', serif;
  font-size: 1.1rem;
}

.lightbox {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.45);
  backdrop-filter: blur(4px);
  z-index: 2000;
  display: flex;
  align-items: center; justify-content: center;
  padding: 1.5rem;
}
.modal-wrap {
  background: white;
  padding: 10px 10px 40px;
  border-radius: 4px;
  box-shadow: 0 40px 100px rgba(0,0,0,0.4);
  position: relative;
  animation: modalScale 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  max-width: 90vw;
}
@keyframes modalScale {
  from { transform: scale(0.9); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}
.modal-box { position: relative; }
.modal-img {
  max-width: 100%;
  max-height: 75vh;
  display: block;
  border-radius: 2px;
}
.modal-close {
  position: absolute;
  top: -15px; right: -15px;
  width: 32px; height: 32px;
  background: white;
  border: 1px solid var(--border);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  z-index: 2100;
  transition: all 0.2s;
}
.modal-close:hover { transform: scale(1.1); color: var(--pink-500); }
.x-icon { font-size: 1.2rem; font-weight: bold; line-height: 1; }

.fade-enter-active, .fade-leave-active { transition: opacity 0.3s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>