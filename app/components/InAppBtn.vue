<!-- <script setup>
import { ref } from 'vue'

const APP_LINK = 'buyandship://routing?screen=/login'
const FALLBACK_LINK = 'https://qr.page/g/tSLYydLM2e'
const TIMEOUT = 2000

const hasOpened = ref(false)

function openApp() {
  const iframe = document.createElement('iframe')
  iframe.style.display = 'none'
  iframe.src = APP_LINK
  document.body.appendChild(iframe)

  const startTime = Date.now()

  window.onblur = () => {
    hasOpened.value = true
  }

  setTimeout(() => {
    if (!hasOpened.value && Date.now() - startTime < TIMEOUT + 100) {
      window.location.href = FALLBACK_LINK
    }
    document.body.removeChild(iframe)
  }, TIMEOUT)
}
</script>

<template>
  <div>
    <button btn @click="openApp">
      Tracking App
    </button>
    <a underline underline-blue href="buyandship://routing?screen=/login">
      強開 App
    </a>
  </div>
</template> -->

<!-- <script setup>
import { ref } from 'vue'

const APP_LINK = 'buyandship://routing?screen=/login'
const FALLBACK_LINK = 'https://qr.page/g/tSLYydLM2e'
const TIMEOUT = 3000

const hasOpened = ref(false)

function openApp() {
  // 嘗試直接打開應用程式
  window.location.href = APP_LINK

  const startTime = Date.now()

  // 使用 visibilitychange 事件來檢測頁面是否隱藏（可能表示應用程式已打開）
  document.addEventListener('visibilitychange', () => {
    if (document.hidden) {
      hasOpened.value = true
    }
  })

  // 設置 fallback
  setTimeout(() => {
    if (!hasOpened.value && Date.now() - startTime < TIMEOUT + 100) {
      window.location.href = FALLBACK_LINK
    }
  }, TIMEOUT)
}
</script>

<template>
  <div>
    <button btn @click="openApp">
      Tracking App
    </button>
    <a underline underline-blue :href="APP_LINK">
      強開 App
    </a>
  </div>
</template> -->

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

const APP_LINK = 'buyandship://routing?screen=/login'
const FALLBACK_LINK = 'https://qr.page/g/tSLYydLM2e'
const TIMEOUT = 3000

const hasOpened = ref(false)
const isProcessing = ref(false)
let timeoutId = null

function resetState() {
  hasOpened.value = false
  isProcessing.value = false
  if (timeoutId) {
    clearTimeout(timeoutId)
    timeoutId = null
  }
}

function openApp() {
  if (isProcessing.value)
    return

  resetState()
  isProcessing.value = true

  window.location.href = APP_LINK

  const startTime = Date.now()

  timeoutId = setTimeout(() => {
    if (!hasOpened.value && Date.now() - startTime < TIMEOUT + 100) {
      window.location.href = FALLBACK_LINK
    }
    isProcessing.value = false
  }, TIMEOUT)
}

function handleVisibilityChange() {
  if (document.hidden) {
    hasOpened.value = true
    isProcessing.value = false
  }
}

onMounted(() => {
  document.addEventListener('visibilitychange', handleVisibilityChange)
})

onUnmounted(() => {
  document.removeEventListener('visibilitychange', handleVisibilityChange)
  resetState()
})
</script>

<template>
  <div>
    <button btn :disabled="isProcessing" @click="openApp">
      {{ isProcessing ? '處理中...' : 'Tracking App' }}
    </button>
    <a underline underline-blue :href="APP_LINK">
      強開 App
    </a>
  </div>
</template>
