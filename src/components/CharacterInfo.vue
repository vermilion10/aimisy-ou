<script setup>
import { ref } from 'vue'
import { 
  Star, Flame, Sword, Ghost, MapPin, Users 
} from 'lucide-vue-next'

const selectedImg = ref(null)

const stats = [
    { icon: Star, label: "Rarity", value: "5-Star", color: "text-yellow" },
    { icon: Flame, label: "Element", value: "Fusion", color: "text-orange" },
    { icon: Sword, label: "Weapon", value: "Sword", color: "text-primary" },
    { icon: Ghost, label: "Class", value: "Natural Resonator", color: "text-purple" },
    { icon: MapPin, label: "Region", value: "Lahai-Roi", color: "text-blue" },
    { icon: Users, label: "Affiliation", value: "Roya Tribe", color: "text-emerald" },
];

const charImgs = [
  'aemeath/Aemeath Idol.jpg',
  'aemeath/Aemeath Decisions.jpg',
  'aemeath/Aemeath Lantern Festival.jpg',
  'aemeath/Aemeath Music.jpg',
  'aemeath/Aemeath PV.jpg',
  'aemeath/Aemeath Plane.jpg',
  'aemeath/Aemeath Promo.jpg',
  'aemeath/Aemeath Wuwa Together.jpg'
].map(p => `${import.meta.env.BASE_URL}${p}`)

const BURST_OFFSETS = [
  { x: -15, y: -20, r: -4 }, { x: 20, y: 15, r: 5 }, { x: -30, y: 10, r: -3 },
  { x: 25, y: -25, r: 6 }, { x: -10, y: 30, r: -5 }, { x: 35, y: -10, r: 4 },
  { x: -20, y: -35, r: 7 }, { x: 15, y: 25, r: -6 }
]

function getBurstStyle(i) {
  const off = BURST_OFFSETS[i % BURST_OFFSETS.length]
  return {
    '--off-x': off.x + 'px',
    '--off-y': off.y + 'px',
    '--rot': off.r + 'deg',
    transitionDelay: (i * 0.05) + 's'
  }
}
</script>

<template>
  <section class="char-info container" id="about">
    <div class="char-header text-center reveal">
      <span class="tag"> </span>
      <h2 class="section-title">The Digital Ghost</h2>
    </div>

    <div class="char-grid">
      <div class="char-bio reveal">
        <div class="bio-accent"></div>
        <div class="bio-content">
          <p>
            <strong>Aemeath</strong> (爱弥斯) is a Frostlands native from the <strong>Roya Tribe</strong> who was once a Synchronist at Startorch Academy.
            She became the Exostrider's Resonator after overclocking in order to resonate with it,
            causing her to lose her body.
          </p>
          <p>
            She now roams throughout Lahai-Roi as a <em class="text-primary-color">digital ghost</em> invisible to everyone
            around her — except for Rover, whom she met during her childhood.
          </p>
        </div>
      </div>

      <div class="stats-grid">
        <div 
          v-for="(stat, i) in stats" 
          :key="stat.label"
          class="stat-card reveal"
          :style="{ transitionDelay: (0.1 * i) + 's' }"
        >
          <component :is="stat.icon" class="stat-icon" :class="stat.color" />
          <p class="stat-label">{{ stat.label }}</p>
          <p class="stat-value">{{ stat.value }}</p>
        </div>
      </div>
    </div>

    <div class="char-gallery-section reveal">
      <div class="char-burst-container">
        <div
          v-for="(img, i) in charImgs"
          :key="i"
          class="char-burst-item"
          :style="getBurstStyle(i)"
          @click="selectedImg = img"
        >
          <div class="char-polaroid">
            <img :src="img" :alt="`Aemeath ${i + 1}`" loading="lazy" />
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

<style scoped>
.char-info {
  padding: 4rem 0;
  position: relative;
  overflow: visible;
}

.char-header {
  margin-bottom: 4rem;
}

.char-grid {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 3rem;
  margin-bottom: 4rem;
}

@media (max-width: 992px) {
  .char-grid {
    grid-template-columns: 1fr;
  }
}

.char-bio {
  position: relative;
  display: flex;
  gap: 1.5rem;
}
.bio-accent {
  width: 4px;
  background: linear-gradient(to bottom, var(--pink-500), var(--pink-300), transparent);
  border-radius: 4px;
}
.bio-content {
  font-size: 1.1rem;
  color: var(--muted-foreground);
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}
.bio-content p {
  margin-bottom: 1.5rem;
}
.bio-content strong {
  color: var(--text);
  font-weight: 600;
}
.text-primary-color {
  color: var(--pink-500);
  font-style: italic;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 1rem;
}
.stat-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 1rem;
  padding: 1.5rem;
  text-align: center;
  transition: all 0.3s ease;
}
.stat-card:hover {
  transform: translateY(-5px);
  border-color: var(--pink-300);
  box-shadow: var(--shadow);
}
.stat-icon {
  width: 1.5rem;
  height: 1.5rem;
  margin: 0 auto 0.75rem;
}
.stat-label {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--muted-foreground);
  margin-bottom: 0.25rem;
}
.stat-value {
  font-size: 1rem;
  font-weight: 700;
  color: var(--text);
}

.text-yellow { color: #f59e0b; }
.text-orange { color: #f97316; }
.text-primary { color: var(--pink-500); }
.text-purple { color: #a855f7; }
.text-blue { color: #3b82f6; }
.text-emerald { color: #10b981; }

.char-gallery-section {
  margin-top: 4rem;
  padding-bottom: 2rem;
}
.char-burst-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2rem;
  max-width: 1000px;
  margin: 0 auto;
}
.char-burst-item {
  transform: translate(var(--off-x), var(--off-y)) rotate(var(--rot));
  transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  cursor: pointer;
}
.char-burst-item:hover {
  transform: translate(var(--off-x), var(--off-y)) rotate(0deg) scale(1.08);
  z-index: 10;
}
.char-polaroid {
  background: white;
  padding: 8px 8px 24px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.08);
  border-radius: 2px;
  border: 1px solid rgba(0,0,0,0.03);
}
.char-polaroid img {
  width: 200px;
  height: 150px;
  object-fit: cover;
  display: block;
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

@media (max-width: 768px) {
  .char-burst-container { gap: 1rem; }
  .char-polaroid img { width: 140px; height: 105px; }
}
</style>
