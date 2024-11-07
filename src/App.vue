<script setup>
import { RouterView } from 'vue-router'
import { onMounted, computed } from 'vue'
import { useAuth } from '@/composables/useAuth'

// 현재 날짜와 시간을 가져오기
const currentDate = new Date()

// 날짜와 시간을 문자열로 포맷팅 AM/PM 12시간 형식
const hour = `${currentDate.getHours() % 12 || 12}`
const minute = `${currentDate.getMinutes()}`.padStart(2, '0')

// 다크 모드 여부를 계산하는 computed 프로퍼티
const isDarkMode = computed(() => {
  if (typeof localStorage !== 'undefined') {
    return localStorage.getItem('color-theme') === 'dark'
  }
  return false // localStorage가 undefined일 경우 false 반환
})

// 앱 로드 시 초기 인증 상태 설정
onMounted(() => {
  const { checkAuth } = useAuth()
  checkAuth()
})
</script>

<script>
import { useAuth } from '@/composables/useAuth'
import { onMounted } from 'vue'

export default {
  setup() {
    const { checkAuth } = useAuth()

    onMounted(() => {
      checkAuth() // 앱 로드 시 초기 인증 상태 설정
    })
  },
}
</script>

<template>
  <div class="frame">
    <div class="content scroll-container">
      <div class="sticky top-0 status-bar">
        <div
          class="clock text-dark dark:text-white backdrop-blur-3xl	"
        >
          <div
            :class="
              ('hour')
            "
          >
            {{ hour }}:
          </div>
          <div
            :class="
              ('minute')
            "
          >
            {{ minute }}
          </div>
        </div>
        <img
          class="block dark:hidden mr-2 backdrop-blur-3xl"
          src="https://cdn.lunaiz.com/my_uni_assets/statusbar-l.svg"
          alt=""
        />
        <img
          class="hidden dark:block mr-2 backdrop-blur-3xl"
          src="https://cdn.lunaiz.com/my_uni_assets/statusbar-d.svg"
          alt=""
        />
      </div>
      <RouterView />
    </div>
  </div>
</template>
