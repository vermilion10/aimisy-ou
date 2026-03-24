<template>
  <section class="fleet-section" id="music">
    <div class="container">
      <h2 class="section-title reveal">Fleet Snowfluff</h2>
      <p class="section-sub reveal">Aemeath's Vocal Identity</p>

      <div class="fleet-card glass-card reveal">
        <div class="fleet-main">
          <div class="fleet-img-wrap">
            <img :src="fleetImgs[activeFleet]" alt="Fleet Snowfluff" class="fleet-main-img" />
          </div>
          <div class="fleet-info">
            <h3 class="fleet-name">Fleet Snowfluff</h3>
            <p class="fleet-role">🎤 Wuthering Waves · In-Game Singer</p>
            <div class="track-list">
              <div v-for="track in tracks" :key="track" class="track-item">
                <span class="track-note">♪</span>
                <span>{{ track }}</span>
              </div>
            </div>
            <p class="fleet-bio">
              The ethereal vocal persona behind Aemeath's theme songs in Wuthering Waves.
              Fleet Snowfluff EP dropped January 19, 2026 — featuring <em>Fallen Petals</em>
              and <em>Indigo Universe</em>.
            </p>
          </div>
        </div>

        <div class="fleet-thumbs">
          <img
            v-for="(img, i) in fleetImgs"
            :key="i"
            :src="img"
            :class="{ active: activeFleet === i }"
            @click="activeFleet = i"
            :alt="`Fleet ${i + 1}`"
          />
        </div>
      </div>

      <div class="spotify-section container">
        <div class="header text-center reveal">
          <span class="tag"> </span>
          <h2 class="section-title">Fleet Snowfluff</h2>
          <p class="section-subtitle">Aemeath's original game soundtrack</p>
        </div>

        <div class="spotify-grid">
          <div class="spotify-card reveal" style="transition-delay: 0.2s">
            <h3 class="card-tag">🎤 Fleet Snowfluff — Artist</h3>
            <div class="iframe-wrap">
              <iframe
                src="https://open.spotify.com/embed/artist/5TaFcGoCPhiGvEDGalmewH?utm_source=generator&theme=0"
                width="100%"
                height="352"
                frameBorder="0"
                allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
                loading="lazy"
              ></iframe>
            </div>
          </div>
        </div>
      </div>

      <div class="fleet-burst-container reveal">
        <div
          v-for="(img, i) in fleetImgs"
          :key="i"
          class="fleet-burst-item"
          :style="getBurstStyle(i)"
          @click="selectedImg = img"
        >
          <div class="fleet-polaroid">
            <img :src="img" :alt="`Fleet ${i + 1}`" loading="lazy" />
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
import { ref } from 'vue'

const baseUrl = import.meta.env.BASE_URL
const activeFleet = ref(0)
const selectedImg = ref(null)
const fleetImgs = [
  'fleetsnowfluff/flt01.jpg', 'fleetsnowfluff/flt02.jpg',
  'fleetsnowfluff/flt03.jpg', 'fleetsnowfluff/flt04.jpg',
  'fleetsnowfluff/flt05.jpg', 'fleetsnowfluff/flt06.jpg',
  'fleetsnowfluff/flt07.jpg', 'fleetsnowfluff/flt08.jpg',
  'fleetsnowfluff/flt09.jpg', 'fleetsnowfluff/flt10.jpg',
  'fleetsnowfluff/flt11.jpg',
].map(p => `${import.meta.env.BASE_URL}${p}`)

const BURST_OFFSETS = [
  { x: -10, y: -20, r: -5 }, { x: 15, y: 10, r: 4 }, { x: -20, y: 15, r: -3 },
  { x: 25, y: -15, r: 6 }, { x: -5, y: -25, r: -4 }, { x: 30, y: 20, r: 5 },
  { x: -15, y: 25, r: -6 }, { x: 10, y: -10, r: 3 }, { x: 20, y: 5, r: -4 },
  { x: -25, y: -5, r: 7 }, { x: 5, y: 30, r: -5 }
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

const tracks = ['A Small Miracle', 'Indigo Universe', 'Fallen Petals', "Voyaging Star's Farewell"]
</script>

<style scoped>
.fleet-section {
  padding: 6rem 0 10rem;
  position: relative;
  z-index: 2;
  overflow: visible;
}

.fleet-card { overflow: hidden; margin-bottom: 2.5rem; }

.fleet-main {
  display: flex;
  flex-direction: column;
}
@media (min-width: 560px) {
  .fleet-main { flex-direction: row; }
  .fleet-img-wrap { width: 200px; flex-shrink: 0; }
}

.fleet-img-wrap { position: relative; }
.fleet-main-img {
  width: 100%; height: 220px;
  object-fit: cover;
  object-position: top;
  display: block;
}
@media (min-width: 560px) {
  .fleet-main-img { height: 100%; min-height: 240px; }
}

.fleet-info { padding: 1.4rem; flex: 1; }
.fleet-name {
  font-family: 'Playfair Display', serif;
  font-size: 1.5rem;
  color: var(--text);
  margin-bottom: 0.25rem;
}
.fleet-role {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--pink-500);
  font-weight: 600;
  margin-bottom: 1rem;
}
.track-list { margin-bottom: 1rem; }
.track-item {
  display: flex; align-items: center; gap: 0.5rem;
  font-size: 0.84rem;
  color: var(--text-muted);
  padding: 0.2rem 0;
  border-bottom: 1px solid var(--border);
}
.track-note { color: var(--pink-400); }

.fleet-bio {
  font-size: 0.82rem;
  color: var(--text-muted);
  line-height: 1.7;
}
.fleet-bio em { color: var(--pink-500); font-style: normal; font-weight: 600; }

.fleet-thumbs {
  display: flex;
  gap: 0.4rem;
  padding: 0.8rem;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  border-top: 1px solid var(--border);
}
.fleet-thumbs::-webkit-scrollbar { display: none; }
.fleet-thumbs img {
  width: 50px; height: 50px;
  object-fit: cover;
  border-radius: 8px;
  cursor: pointer;
  border: 2px solid transparent;
  flex-shrink: 0;
  opacity: 0.6;
  transition: opacity 0.2s, border-color 0.2s, transform 0.2s;
}
.fleet-thumbs img.active,
.fleet-thumbs img:hover {
  opacity: 1;
  border-color: var(--pink-500);
  transform: scale(1.06);
}

.spotify-section {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.spotify-grid {
  width: 100%;
  max-width: 1200px;
  margin: 3rem auto;
}
.spotify-card {
  width: 100%;
}
@media (max-width: 768px) {
  .spotify-card {
    max-width: 90%;
    margin: 0 auto;
  }
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

.fleet-burst-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1.5rem;
  padding: 2rem 1rem;
  margin-top: 3rem;
  max-width: 900px;
  margin-left: auto;
  margin-right: auto;
}

.fleet-burst-item {
  transform: translate(var(--off-x), var(--off-y)) rotate(var(--rot));
  transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275), opacity 0.5s ease;
  cursor: pointer;
}

.fleet-burst-item:hover {
  transform: translate(var(--off-x), var(--off-y)) rotate(0deg) scale(1.1);
  z-index: 10;
}

.fleet-polaroid {
  background: white;
  padding: 6px 6px 18px;
  box-shadow: 0 8px 25px rgba(0,0,0,0.08);
  border-radius: 2px;
  border: 1px solid rgba(0,0,0,0.03);
}

.fleet-polaroid img {
  width: 140px;
  height: 140px;
  object-fit: cover;
  display: block;
  border-radius: 1px;
}

@media (max-width: 768px) {
  .fleet-burst-container { gap: 0.8rem; }
  .fleet-polaroid img { width: 100px; height: 100px; }
  .fleet-burst-item {
     transform: translate(calc(var(--off-x) * 0.5), calc(var(--off-y) * 0.5)) rotate(var(--rot));
  }
}
</style>
