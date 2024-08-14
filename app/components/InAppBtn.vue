<script setup>
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
      點擊開啟 App
    </button>
  </div>
</template>
