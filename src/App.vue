<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-gradient-to-b from-amber-100 to-orange-200 text-center">
    <!-- 달 이미지 -->
    <div class="relative">
      <div
          class="w-48 h-48 bg-yellow-300 rounded-full shadow-lg animate-pulse"
          style="box-shadow: 0 0 60px rgba(255, 221, 0, 0.6)"
      ></div>
      <span class="absolute inset-0 flex items-center justify-center text-3xl font-bold text-yellow-800">🌕</span>
    </div>

    <!-- 제목 -->
    <h1 class="mt-8 text-4xl md:text-5xl font-bold text-amber-900 drop-shadow-lg">
      한가위 잘 보내세요 🌾
    </h1>

    <!-- 부제 -->
    <p class="mt-4 text-lg text-amber-800">
      보름달처럼 풍성하고 따뜻한 추석 되세요!
    </p>

    <!-- 버튼 -->
    <button
        class="mt-8 bg-amber-700 hover:bg-amber-800 text-white py-3 px-6 rounded-2xl text-lg shadow-md transition duration-300"
        @click="handleClick"
    >
      🎁 감사 인사 전하기
    </button>

    <!-- 푸터 -->
    <footer class="mt-12 text-sm text-amber-700 opacity-70">
      © {{ currentYear }} Yeonjae’s Chuseok Page
    </footer>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const currentYear = new Date().getFullYear()

const handleClick = () => {
  alert('풍성한 한가위 보내세요! 🌕💛')
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sunflower:wght@500&display=swap');

* {
  font-family: 'Sunflower', sans-serif;
}

html,
body {
  margin: 0;
  padding: 0;
}
</style>
