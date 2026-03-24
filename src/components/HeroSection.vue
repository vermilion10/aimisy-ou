<template>
  <section class="hero" id="home">
    <div class="hero-bg-orbs">
      <div class="orb orb-1"></div>
      <div class="orb orb-2"></div>
      <div class="orb orb-3"></div>
    </div>

    <div class="decor decor-1 animate-sparkle">✨</div>
    <div class="decor decor-2 animate-float">💗</div>
    <div class="decor decor-3 animate-sparkle">✨</div>

    <div class="hero-content container">
      <div class="hero-img-wrap">
        <img
          :src="`${baseUrl}Aemeath_Splash_Art.webp`"
          alt="Aemeath"
          class="hero-splash"
          :style="{ transform: `translateY(${scrollY * 0.1}px) scale(1)` }"
        />
      </div>

      <p class="hero-tag">WUTHERING WAVES</p>
      <h1 class="hero-name">Aemeath</h1>
      <div class="hero-divider"></div>
      <p class="hero-quote">"Did you see me?"</p>
      
      <div class="hero-seals">
        <span v-for="i in 5" :key="i" :style="{ animationDelay: i * 0.2 + 's' }">
          {{ i % 2 === 0 ? '✨' : '🦭' }}
        </span>
      </div>

      <div class="hero-cta">
        <a href="#timeline" class="btn-primary">📷 Key Moments</a>
        <a href="#music" class="btn-ghost">🎵 Music</a>
      </div>
    </div>

    <div class="scroll-hint">
      <div class="scroll-mouse">
        <div class="scroll-dot"></div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

const baseUrl = import.meta.env.BASE_URL
const scrollY = ref(0)
function handleScroll() { scrollY.value = window.scrollY }

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
})
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<style scoped>
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-bottom: 6rem;
  overflow: visible;
}

.hero-bg-orbs {
  position: absolute;
  inset: 0;
  z-index: 0;
  overflow: hidden;
  pointer-events: none;
}
.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.4;
}
.orb-1 { top: 20%; left: 20%; width: 400px; height: 400px; background: var(--pink-300); }
.orb-2 { bottom: 20%; right: 20%; width: 350px; height: 350px; background: var(--accent); }
.orb-3 { top: 50%; left: 50%; transform: translate(-50%, -50%); width: 250px; height: 250px; background: var(--pink-200); }

.decor {
  position: absolute;
  z-index: 5;
  font-size: 1.5rem;
  pointer-events: none;
}
.decor-1 { top: 20%; right: 10%; color: var(--pink-500); }
.decor-2 { bottom: 30%; left: 8%; opacity: 0.6; }
.decor-3 { top: 35%; left: 12%; font-size: 1.2rem; opacity: 0.4; }

.hero-img-wrap {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 2rem;
}
.hero-splash {
  width: 100%;
  max-width: 420px;
  height: auto;
  object-fit: contain;
  transition: transform 0.1s linear;
  will-change: transform;
  filter: drop-shadow(0 15px 40px rgba(214, 58, 114, 0.15));
}

.hero-content {
  position: relative;
  z-index: 10;
  text-align: center;
}

.hero-tag {
  font-size: 0.75rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--pink-500);
  opacity: 0.7;
  font-weight: 600;
  margin-bottom: 1rem;
}

.hero-name {
  font-family: 'Playfair Display', serif;
  font-size: clamp(4rem, 15vw, 8rem);
  line-height: 1;
  color: var(--text);
  margin-bottom: 0.5rem;
  letter-spacing: -0.02em;
}

.hero-divider {
  width: 240px;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--pink-500), transparent);
  margin: 1.5rem auto;
}

.hero-quote {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  font-size: clamp(1rem, 4vw, 1.25rem);
  color: var(--muted-foreground);
  margin-bottom: 2rem;
}

.hero-seals {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 2.5rem;
}
.hero-seals span {
  font-size: 1.5rem;
  display: inline-block;
  animation: sealJump 2s ease-in-out infinite;
}
@keyframes sealJump {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
}

.hero-cta {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
}

.btn-primary {
  background: var(--pink-500);
  color: #fff;
  padding: 0.8rem 2rem;
  border-radius: 50px;
  text-decoration: none;
  font-weight: 600;
  box-shadow: 0 10px 30px rgba(214, 58, 114, 0.3);
  transition: transform 0.3s, box-shadow 0.3s;
}
.btn-primary:hover { transform: translateY(-3px); box-shadow: 0 15px 40px rgba(214, 58, 114, 0.4); }

.btn-ghost {
  border: 2px solid var(--border);
  color: var(--pink-500);
  padding: 0.8rem 2rem;
  border-radius: 50px;
  text-decoration: none;
  font-weight: 500;
  background: var(--surface);
  transition: all 0.3s;
}
.btn-ghost:hover { border-color: var(--pink-400); transform: translateY(-3px); }

.scroll-hint {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
}
.scroll-mouse {
  width: 26px;
  height: 42px;
  border: 2px solid var(--pink-300);
  border-radius: 15px;
  display: flex;
  justify-content: center;
  padding-top: 8px;
}
.scroll-dot {
  width: 5px;
  height: 5px;
  background: var(--pink-500);
  border-radius: 50%;
  animation: scrollAnim 2s infinite;
}
@keyframes scrollAnim {
  0% { transform: translateY(0); opacity: 1; }
  80% { transform: translateY(18px); opacity: 0; }
  100% { transform: translateY(0); opacity: 0; }
}
</style>
