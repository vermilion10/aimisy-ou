<template>
  <nav class="navbar" :class="{ scrolled: isScrolled }">
    <div class="nav-inner">
      <div class="nav-logo">
        <span class="seal-icon">🦭</span>
        <span class="logo-text">Aemeath</span>
      </div>
      <div class="nav-right">
        <div class="nav-links">
          <a href="#home">Home</a>
          <a href="#timeline">Key Moments</a>
          <a href="#music">Music</a>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isScrolled = ref(false)
function onScroll() { isScrolled.value = window.scrollY > 60 }
onMounted(() => window.addEventListener('scroll', onScroll))
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  padding: 0.9rem 1.5rem;
  transition: background 0.4s, backdrop-filter 0.4s, box-shadow 0.4s;
}
.navbar.scrolled {
  background: var(--navbar-bg);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  box-shadow: 0 1px 0 var(--border);
}
.nav-inner {
  max-width: 480px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
@media (min-width: 768px) { .nav-inner { max-width: 760px; } }

.nav-logo { display: flex; align-items: center; gap: 0.45rem; }
.seal-icon { font-size: 1.2rem; }
.logo-text {
  font-family: 'Playfair Display', serif;
  font-size: 1.05rem;
  color: var(--pink-500);
  font-weight: 700;
}

.nav-right { display: flex; align-items: center; gap: 1rem; }
.nav-links { display: flex; gap: 1.2rem; }
.nav-links a {
  color: var(--text-muted);
  text-decoration: none;
  font-size: 0.8rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  transition: color 0.25s;
  font-weight: 500;
}
.nav-links a:hover { color: var(--pink-500); }

.theme-toggle {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 50%;
  width: 34px; height: 34px;
  display: flex; align-items: center; justify-content: center;
  cursor: pointer;
  font-size: 1rem;
  transition: background 0.25s, transform 0.25s, box-shadow 0.25s;
  flex-shrink: 0;
}
.theme-toggle:hover {
  transform: rotate(20deg) scale(1.1);
  box-shadow: var(--shadow);
}
</style>
