<template>
  <div id="wrapper" class="seal-pattern">
    <FloatingSeals />
    <MusicPlayer />
    <NavBar />
    <HeroSection />
    <CharacterInfo />
    <StoryTimeline />
    <FleetSpotify />
    <FooterSection />
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import NavBar from './components/NavBar.vue'
import MusicPlayer from './components/MusicPlayer.vue'
import FloatingSeals from './components/FloatingSeals.vue'
import HeroSection from './components/HeroSection.vue'
import CharacterInfo from './components/CharacterInfo.vue'
import StoryTimeline from './components/StoryTimeline.vue'
import FleetSpotify from './components/FleetSpotify.vue'
import FooterSection from './components/FooterSection.vue'

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('visible')
          observer.unobserve(e.target)
        }
      })
    },
    { threshold: 0.08, rootMargin: '0px 0px -40px 0px' }
  )
  document.querySelectorAll('.reveal').forEach(el => observer.observe(el))
})
</script>

<style>
#wrapper {
  position: relative;
  min-height: 100vh;
  background: var(--bg);
  transition: background 0.4s ease;
}
</style>
