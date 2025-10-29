<template>
  <div class="chuseok-wrap">
    <div class="sky">
      <div class="moon" :class="{ glow: moonGlow }">
        <div class="rabbit">🐇</div>
      </div>

      <div
          v-for="(lantern, i) in lanterns"
          :key="i"
          class="lantern"
          :style="{
          left: lantern.x + '%',
          top: lantern.y + '%',
          transform: `translate(-50%, -50%) scale(${lantern.scale})`
        }"
      >
        <div class="lantern-body"></div>
        <div class="lantern-glow"></div>
      </div>

      <div class="shooting-star" v-if="showStar"></div>
    </div>

    <div class="content">
      <h1>풍성한 한가위 되세요 🎑</h1>

      <div class="greeting-card">
        <p class="greeting">{{ currentGreeting }}</p>

        <div class="controls">
          <button @click="prevGreeting">이전 덕담</button>
          <button @click="nextGreeting">다음 덕담</button>
          <button @click="randomGreeting">랜덤 덕담</button>
          <button @click="toggleMoonGlow">달 반짝임 {{ moonGlow ? '끄기' : '켜기' }}</button>
        </div>
      </div>

      <p class="hint">원하시면 덕담을 추가해 주세요 — 아래 입력 후 추가 버튼을 누르세요.</p>
      <div class="add-greeting">
        <input v-model="newGreeting" placeholder="새 덕담을 입력하세요" @keyup.enter="addGreeting" />
        <button @click="addGreeting">추가</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, computed, onMounted } from 'vue'

type Lantern = {
  x: number
  y: number
  scale: number
  speed: number
}

const moonGlow = ref(true)
const showStar = ref(false)

const greetings = ref<string[]>([
  '가족과 함께 건강하고 행복한 한가위 되세요!',
  '한가위 달처럼 넉넉한 보름달 같은 한 해 되시길 바랍니다.',
  '소원 모두 이루어지길 바랍니다. 즐거운 추석 보내세요!',
  '맛있는 음식과 포근한 시간으로 가득한 한가위 되세요.'
])

const currentIndex = ref(0)
const newGreeting = ref('')

const currentGreeting = computed(() => greetings.value[currentIndex.value])

function nextGreeting() {
  currentIndex.value = (currentIndex.value + 1) % greetings.value.length
}
function prevGreeting() {
  currentIndex.value = (currentIndex.value - 1 + greetings.value.length) % greetings.value.length
}
function randomGreeting() {
  if (greetings.value.length <= 1) return
  let idx = Math.floor(Math.random() * greetings.value.length)
  while (idx === currentIndex.value) idx = Math.floor(Math.random() * greetings.value.length)
  currentIndex.value = idx
}
function addGreeting() {
  const text = newGreeting.value.trim()
  if (text) {
    greetings.value.push(text)
    newGreeting.value = ''
    currentIndex.value = greetings.value.length - 1
  }
}

function toggleMoonGlow() {
  moonGlow.value = !moonGlow.value
}

const lanterns = reactive<Lantern[]>([])
function makeLantern() {
  return {
    x: Math.random() * 100,
    y: 60 + Math.random() * 30,
    scale: 0.6 + Math.random() * 0.8,
    speed: 0.02 + Math.random() * 0.06
  }
}

for (let i = 0; i < 8; i++) {
  lanterns.push(makeLantern())
}

onMounted(() => {
  function step() {
    for (const l of lanterns) {
      l.y -= l.speed
      l.x += Math.sin(Date.now() / 1000 + l.x) * 0.02
      if (l.y < -10) {
        Object.assign(l, makeLantern())
        l.y = 110
      }
    }
    if (Math.random() < 0.003) {
      showStar.value = true
      setTimeout(() => (showStar.value = false), 900)
    }
    requestAnimationFrame(step)
  }
  requestAnimationFrame(step)
})
</script>

<style scoped>


/* 이하 기존 스타일 동일 */
.sky {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.moon {
  position: absolute;
  right: 12%;
  top: 8%;
  width: 160px;
  height: 160px;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, #fff29a 0%, #ffd24d 40%, #f2b600 100%);
  box-shadow: 0 0 40px rgba(255, 210, 80, 0.25);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: box-shadow 0.4s ease, transform 0.4s ease;
}
.moon.glow {
  box-shadow: 0 0 80px rgba(255, 220, 100, 0.6), 0 0 150px rgba(255, 190, 60, 0.2);
  transform: translateY(-4px);
}
.moon .rabbit {
  font-size: 36px;
  transform: translateY(6px);
}

.lantern {
  position: absolute;
  width: 70px;
  height: 110px;
  pointer-events: none;
  transform-origin: center;
  transition: transform 0.2s linear;
}
.lantern-body {
  width: 100%;
  height: 70%;
  border-radius: 40% 40% 30% 30%;
  background: linear-gradient(180deg, #ffefcf 0%, #ffcf7a 60%, #ffb24a 100%);
  box-shadow: 0 8px 22px rgba(255,160,60,0.12);
  position: relative;
}
.lantern-body::after {
  content: '';
  position: absolute;
  bottom: -12px;
  left: 50%;
  transform: translateX(-50%);
  width: 6px;
  height: 24px;
  background: linear-gradient(#b36a1b, #7a3d10);
  border-radius: 3px;
}
.lantern-glow {
  position: absolute;
  inset: 0;
  filter: blur(8px);
  opacity: 0.45;
  background: radial-gradient(circle at 50% 35%, rgba(255,240,190,0.9) 0%, rgba(255,170,80,0.0) 45%);
  border-radius: inherit;
}

.shooting-star {
  position: absolute;
  top: 10%;
  left: 15%;
  width: 200px;
  height: 2px;
  transform: rotate(-25deg);
  background: linear-gradient(90deg, rgba(255,255,255,0.0) 0%, rgba(255,255,255,0.9) 45%, rgba(255,255,255,0.0) 100%);
  animation: starMove 0.9s linear forwards;
}
@keyframes starMove {
  from { transform: translateX(-80%) rotate(-25deg); opacity: 1 }
  to { transform: translateX(120%) rotate(-25deg); opacity: 0 }
}

.content {
  position: relative;
  z-index: 10;
  width: min(820px, 92%);
  text-align: center;
  padding: 40px 24px;
  backdrop-filter: blur(6px) saturate(1.1);
}

h1 {
  margin: 0 0 12px 0;
  font-size: 32px;
  letter-spacing: 0.6px;
}

.greeting-card {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255,255,255,0.06);
  padding: 18px;
  border-radius: 12px;
  box-shadow: 0 8px 30px rgba(2,8,23,0.6);
}
.greeting {
  font-size: 20px;
  margin: 6px 0 12px 0;
  min-height: 56px;
}
.controls {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  justify-content: center;
}
.controls button {
  background: linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,255,255,0.03));
  border: 1px solid rgba(255,255,255,0.08);
  color: #fff;
  padding: 8px 12px;
  border-radius: 8px;
  cursor: pointer;
}
.controls button:hover { transform: translateY(-3px) }

.hint { margin-top: 12px; opacity: 0.85 }
.add-greeting { display:flex; gap:8px; justify-content:center; margin-top:8px }
.add-greeting input { padding:8px 10px; border-radius:8px; border:1px solid rgba(255,255,255,0.08); background: rgba(0,0,0,0.25); color:#fff; min-width:300px }
.add-greeting button { padding:8px 12px; border-radius:8px; border:none; cursor:pointer }

@media (max-width: 640px) {
  .moon { width: 110px; height: 110px }
  .lantern { width: 54px; height: 86px }
  h1 { font-size: 22px }
}
</style>
