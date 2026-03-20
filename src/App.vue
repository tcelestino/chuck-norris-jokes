<script setup>
import { ref, onMounted } from 'vue'

const joke = ref('')
const isLoading = ref(false)
const visible = ref(false)

async function fetchJoke() {
  if (isLoading.value) return

  visible.value = false
  isLoading.value = true

  try {
    await new Promise(r => setTimeout(r, 300))

    const response = await fetch('https://api.chucknorris.io/jokes/random')
    if (!response.ok) throw new Error('API error')

    const data = await response.json()
    joke.value = data.value
  } catch {
    joke.value = "Chuck Norris doesn't make errors. The universe makes exceptions for him."
  } finally {
    isLoading.value = false
    await new Promise(r => setTimeout(r, 50))
    visible.value = true
  }
}

onMounted(fetchJoke)
</script>

<template>
  <div class="app">
    <div class="noise" aria-hidden="true"></div>
    <div class="vignette" aria-hidden="true"></div>

    <main class="main">
      <div class="corner corner--tl" aria-hidden="true"></div>
      <div class="corner corner--tr" aria-hidden="true"></div>
      <div class="corner corner--bl" aria-hidden="true"></div>
      <div class="corner corner--br" aria-hidden="true"></div>

      <div class="content">
        <header class="header">
          <div class="badge" aria-hidden="true">
            <span class="badge__star">★</span>
          </div>

          <h1 class="title">
            <span class="title__top">CHUCK</span>
            <span class="title__bottom">NORRIS</span>
          </h1>

          <div class="rule" aria-hidden="true">
            <div class="rule__line"></div>
            <div class="rule__ornament">◆</div>
            <div class="rule__line"></div>
          </div>

          <p class="label">JOKES</p>
        </header>

        <section
          class="joke"
          :class="{ 'joke--visible': visible }"
          aria-live="polite"
          aria-atomic="true"
        >
          <span class="joke__quote joke__quote--open" aria-hidden="true">"</span>

          <div class="joke__body">
            <p v-if="!isLoading && joke" class="joke__text">{{ joke }}</p>
            <div v-else class="joke__loader" aria-label="Loading">
              <span></span>
              <span></span>
              <span></span>
            </div>
          </div>

          <span class="joke__quote joke__quote--close" aria-hidden="true">"</span>
        </section>

        <footer class="actions">
          <button
            class="btn"
            @click="fetchJoke"
            :disabled="isLoading"
            aria-label="Load another Chuck Norris fact"
          >
            <span class="btn__text">{{ isLoading ? 'LOADING...' : 'NEXT FACT' }}</span>
          </button>

          <p class="tagline">One fact at a time. Handle it.</p>
          <p class="api-credit">Powered by <a href="https://api.chucknorris.io" target="_blank" rel="noopener noreferrer">chucknorris.io</a></p>
        </footer>
      </div>
    </main>
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  background-color: #0C0B09;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.noise {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 10;
  opacity: 0.04;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='400' height='400'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='400' height='400' filter='url(%23n)'/%3E%3C/svg%3E");
}

.vignette {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 5;
  background: radial-gradient(ellipse at center, transparent 35%, rgba(0, 0, 0, 0.88) 100%);
}

.main {
  position: relative;
  width: min(90vw, 860px);
  min-height: min(90vh, 680px);
  padding: 3.5rem 3rem;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 20;
}

.corner {
  position: absolute;
  width: 44px;
  height: 44px;
  border-color: #8B6914;
  border-style: solid;
  opacity: 0.5;
}

.corner--tl { top: 0; left: 0; border-width: 1px 0 0 1px; }
.corner--tr { top: 0; right: 0; border-width: 1px 1px 0 0; }
.corner--bl { bottom: 0; left: 0; border-width: 0 0 1px 1px; }
.corner--br { bottom: 0; right: 0; border-width: 0 1px 1px 0; }

.content {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2.5rem;
  text-align: center;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
}

.badge {
  width: 60px;
  height: 60px;
  border: 1.5px solid #8B6914;
  transform: rotate(15deg);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  margin-bottom: 0.5rem;
}

.badge::before {
  content: '';
  position: absolute;
  inset: 4px;
  border: 1px solid rgba(139, 105, 20, 0.3);
}

.badge__star {
  font-size: 1.75rem;
  color: #B8891A;
  transform: rotate(-15deg);
  display: block;
  line-height: 1;
  text-shadow: 0 0 24px rgba(232, 168, 32, 0.5);
}

.title {
  font-family: 'Bebas Neue', sans-serif;
  line-height: 0.88;
  letter-spacing: 0.06em;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title__top {
  font-size: clamp(3.5rem, 10vw, 6.5rem);
  color: #D4B896;
  display: block;
}

.title__bottom {
  font-size: clamp(5rem, 16vw, 10.5rem);
  color: #E8A820;
  display: block;
  text-shadow:
    0 0 80px rgba(232, 168, 32, 0.25),
    0 2px 0 rgba(0, 0, 0, 0.6);
}

.rule {
  display: flex;
  align-items: center;
  gap: 1rem;
  width: 100%;
  max-width: 380px;
}

.rule__line {
  flex: 1;
  height: 1px;
  background: linear-gradient(to right, transparent, #8B6914, transparent);
  opacity: 0.6;
}

.rule__ornament {
  color: #8B6914;
  font-size: 0.55rem;
  opacity: 0.7;
}

.label {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(0.85rem, 2.5vw, 1.1rem);
  letter-spacing: 0.55em;
  color: #5C4D30;
}

.joke {
  position: relative;
  width: 100%;
  max-width: 660px;
  opacity: 0;
  transform: translateY(14px);
  transition: opacity 0.55s ease, transform 0.55s ease;
  display: flex;
  align-items: flex-start;
  gap: 0.25rem;
  min-height: 160px;
  justify-content: center;
}

.joke--visible {
  opacity: 1;
  transform: translateY(0);
}

.joke__quote {
  font-family: 'Special Elite', cursive;
  font-size: clamp(3.5rem, 8vw, 5.5rem);
  color: #8B6914;
  opacity: 0.35;
  line-height: 1;
  flex-shrink: 0;
  user-select: none;
}

.joke__quote--open {
  align-self: flex-start;
  margin-top: -0.6rem;
}

.joke__quote--close {
  align-self: flex-end;
  margin-bottom: -1.8rem;
}

.joke__body {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 120px;
  padding: 0.5rem 0;
}

.joke__text {
  font-family: 'Special Elite', cursive;
  font-size: clamp(1rem, 2.4vw, 1.3rem);
  line-height: 1.8;
  color: #D4B896;
  text-align: center;
}

.joke__loader {
  display: flex;
  gap: 10px;
  align-items: center;
  justify-content: center;
}

.joke__loader span {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #8B6914;
  animation: blink 1.2s ease-in-out infinite;
}

.joke__loader span:nth-child(2) { animation-delay: 0.2s; }
.joke__loader span:nth-child(3) { animation-delay: 0.4s; }

@keyframes blink {
  0%, 100% { opacity: 0.15; transform: scale(0.75); }
  50% { opacity: 1; transform: scale(1.15); }
}

.actions {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
}

.btn {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 1rem;
  letter-spacing: 0.35em;
  color: #0C0B09;
  background: #B8891A;
  border: none;
  padding: 0.9rem 3.25rem;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.1s ease, box-shadow 0.2s ease;
  clip-path: polygon(
    10px 0, calc(100% - 10px) 0,
    100% 10px, 100% calc(100% - 10px),
    calc(100% - 10px) 100%, 10px 100%,
    0 calc(100% - 10px), 0 10px
  );
}

.btn:hover:not(:disabled) {
  background: #E8A820;
  transform: translateY(-2px);
  box-shadow: 0 8px 32px rgba(232, 168, 32, 0.2);
}

.btn:active:not(:disabled) {
  transform: translateY(0);
  box-shadow: none;
}

.btn:disabled {
  opacity: 0.45;
  cursor: not-allowed;
}

.btn__text {
  display: block;
}

.tagline {
  font-family: 'Special Elite', cursive;
  font-size: 0.72rem;
  color: #3D3020;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}

.api-credit {
  font-family: 'Special Elite', cursive;
  font-size: 0.68rem;
  color: #3D3020;
  letter-spacing: 0.08em;
}

.api-credit a {
  color: #6B5220;
  text-decoration: none;
  border-bottom: 1px solid #4A3A18;
  transition: color 0.2s, border-color 0.2s;
}

.api-credit a:hover {
  color: #B8891A;
  border-color: #B8891A;
}

@media (max-width: 600px) {
  .main {
    padding: 2.5rem 1.5rem;
    min-height: 100svh;
  }

  .joke__quote {
    font-size: 3rem;
  }

  .badge {
    width: 48px;
    height: 48px;
  }

  .badge__star {
    font-size: 1.4rem;
  }

  .corner {
    width: 32px;
    height: 32px;
  }
}
</style>
