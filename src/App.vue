<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'

function getWindowInfo() {
  const { innerWidth, innerHeight, outerWidth, outerHeight, devicePixelRatio } = window
  const { width, height, availWidth, availHeight } = window.screen
  return { innerWidth, innerHeight, outerWidth, outerHeight, devicePixelRatio, width, height, availWidth, availHeight }
}

const rawWindowInfo = ref(getWindowInfo())

const computedWindowInfo = computed(() => {
  const { innerWidth, innerHeight, width: logicalWidth, height: logicalHeight, devicePixelRatio } = rawWindowInfo.value
  const physicalWidth = logicalWidth * devicePixelRatio
  const physicalHeight = logicalHeight * devicePixelRatio
  const scale = `${devicePixelRatio * 100}%`
  const viewportSize = `${innerWidth}×${innerHeight}`
  const logicalResolution = `${logicalWidth}×${logicalHeight}`
  const physicalResolution = `${physicalWidth}×${physicalHeight}`
  return {
    viewportSize,
    logicalResolution,
    scale,
    physicalResolution
  }
})

// Refresh info on resize event
onMounted(() => {
  window.addEventListener('resize', () => {
    rawWindowInfo.value = getWindowInfo()
  })
})
</script>

<template>
  <header>
    <p>header</p>
  </header>

  <main>
    <p>{{ computedWindowInfo }}</p>
    <p class="info" v-for="(value, key, index) in rawWindowInfo" :key="index">
      {{ key }}: {{ value }}
    </p>
  </main>

  <footer>
    <!-- Create vertical scrollbar -->
    <p>footer start</p>
    <div class="scrollbar-y"></div>
    <p>footer end</p>
  </footer>
</template>

<style scoped>
.info {
  color: #af3a03;
}

.scrollbar-y {
  height: 1500px;
}
</style>
