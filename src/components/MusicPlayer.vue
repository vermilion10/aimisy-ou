<template>
  <div class="music-player-container" :class="{ 'is-minimized': isMinimized, 'is-playing': isPlaying }">
    <button @click="isMinimized = !isMinimized" class="minimize-toggle" :aria-label="isMinimized ? 'Expand Music' : 'Minimize Music'">
      <component :is="isMinimized ? ChevronLeft : ChevronRight" size="20" />
    </button>

    <div class="music-player-card">
      <div id="youtube-audio-player"></div>

      <button @click="toggleMusic" class="music-btn glass-btn" aria-label="Toggle Music">
        <div class="seal-icon-wrap">
          <span class="seal-emoji">🦭</span>
          <div class="music-waves" v-if="isPlaying">
            <span></span><span></span><span></span>
          </div>
        </div>
        <div class="player-info">
          <span class="track-name">The Moored Boat</span>
          <span class="status-text">{{ isPlaying ? 'Playing' : 'Paused' }}</span>
        </div>
        <div class="control-icon">
          <component :is="isPlaying ? Pause : Play" size="18" />
        </div>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Play, Pause, ChevronLeft, ChevronRight } from 'lucide-vue-next'

const isPlaying = ref(false)
const isMinimized = ref(false)
const player = ref(null)

function onYouTubeIframeAPIReady() {
  player.value = new YT.Player('youtube-audio-player', {
    height: '0',
    width: '0',
    videoId: 'lPQ2AqeMsII',
    playerVars: {
      autoplay: 0,
      loop: 1,
      playlist: 'lPQ2AqeMsII'
    },
    events: {
      onReady: (event) => {
      },
      onStateChange: (event) => {
        if (event.data === YT.PlayerState.PLAYING) isPlaying.value = true
        else if (event.data === YT.PlayerState.PAUSED) isPlaying.value = false
      }
    }
  })
}

function toggleMusic() {
  if (!player.value) return
  if (isPlaying.value) {
    player.value.pauseVideo()
  } else {
    player.value.playVideo()
  }
}

onMounted(() => {
  if (!window.YT) {
    const tag = document.createElement('script')
    tag.src = "https://www.youtube.com/iframe_api"
    const firstScriptTag = document.getElementsByTagName('script')[0]
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag)
    window.onYouTubeIframeAPIReady = onYouTubeIframeAPIReady
  } else {
    onYouTubeIframeAPIReady()
  }
})
</script>

<style scoped>
.music-player-container {
  position: fixed;
  bottom: 1.5rem;
  right: 1.5rem;
  z-index: 1000;
  display: flex;
  align-items: center;
  gap: 0;
  transition: transform 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.is-minimized {
  transform: translateX(calc(100% - 32px));
}

.minimize-toggle {
  width: 32px;
  height: 48px;
  background: white;
  border: 1px solid var(--border);
  border-right: none;
  border-radius: 12px 0 0 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: var(--pink-400);
  box-shadow: -10px 0 30px rgba(0,0,0,0.05);
  transition: all 0.2s;
  z-index: 5;
}
.minimize-toggle:hover {
  color: var(--pink-600);
  background: var(--pink-50);
}

.music-player-card {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(12px);
  border: 1px solid var(--border);
  border-radius: 0 50px 50px 0;
  padding: 0.4rem;
  box-shadow: 0 8px 32px rgba(214, 58, 114, 0.1);
  display: flex;
  align-items: center;
}

.music-btn {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.4rem 1.2rem 0.4rem 0.8rem;
  border-radius: 50px;
  background: transparent;
  border: none;
  transition: all 0.3s ease;
  cursor: pointer;
}

.seal-icon-wrap {
  position: relative;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--pink-50);
  border-radius: 50%;
  font-size: 1.2rem;
  flex-shrink: 0;
}

.music-waves {
  position: absolute;
  inset: -4px;
  border: 2px solid var(--pink-200);
  border-radius: 50%;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 0.8; }
  100% { transform: scale(1.4); opacity: 0; }
}

.player-info {
  display: flex;
  flex-direction: column;
  text-align: left;
  min-width: 100px;
}

.track-name {
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--text);
  line-height: 1.2;
}

.status-text {
  font-size: 0.65rem;
  color: var(--muted-foreground);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.control-icon {
  color: var(--pink-500);
  display: flex;
  align-items: center;
  margin-left: 0.4rem;
}

@media (max-width: 768px) {
  .music-player-container {
    bottom: 1rem;
    right: 1rem;
  }
}
</style>
