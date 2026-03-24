<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-seals">🦭 ✦ 🦭 ✦ 🦭</div>
      <p class="footer-name">Aemeath</p>
      <p class="footer-tag">Digital Ghost · Roya Tribe · Frostlands</p>
      <div class="footer-line"></div>
      <p class="footer-credit">This website is a fan page. All assets belong to Kuro Games.</p>
      <p class="footer-made-with">Made with ❤️ for Aemeath</p>
      <div class="footer-links">
        <a href="https://wutheringwaves.kurogames.com" target="_blank" rel="noopener">Wuthering Waves</a>
        <span>·</span>
        <a href="https://open.spotify.com/artist/5TaFcGoCPhiGvEDGalmewH?si=l4yLUVlKTfyKq3DsIMaDYw" target="_blank" rel="noopener">Spotify</a>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  position: relative; z-index: 2;
  padding: 4rem 0 3rem;
  text-align: center;
  border-top: 1px solid var(--border);
}
.footer-seals {
  font-size: 1.5rem;
  letter-spacing: 0.3rem;
  margin-bottom: 0.8rem;
  animation: bounce 3s ease-in-out infinite;
}
@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-5px); }
}
.footer-name {
  font-family: 'Playfair Display', serif;
  font-size: 2rem;
  color: var(--pink-500);
  margin-bottom: 0.2rem;
}
.footer-tag {
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-bottom: 1.5rem;
}
.footer-line {
  width: 50px; height: 1px;
  background: var(--pink-300);
  margin: 0 auto 1.5rem;
}
.footer-credit {
  font-size: 0.73rem;
  color: var(--text-muted);
  margin-bottom: 0.3rem;
  opacity: 0.8;
}
.footer-made-with {
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--pink-500);
  margin-top: 0.5rem;
}
.footer-links {
  display: flex; justify-content: center; align-items: center;
  gap: 0.5rem;
  margin-top: 1.2rem;
}
.footer-links a {
  color: var(--pink-500);
  text-decoration: none;
  font-size: 0.8rem;
  transition: opacity 0.2s;
}
.footer-links a:hover { opacity: 0.7; }
.footer-links span { color: var(--border); }
</style>
