<script setup lang="ts">
import { onMounted, ref } from 'vue'

function getWindowInfo() {
  const { innerWidth, innerHeight, outerWidth, outerHeight, devicePixelRatio } = window
  const { width, height, availWidth, availHeight } = window.screen
  return { innerWidth, innerHeight, outerWidth, outerHeight, devicePixelRatio, width, height, availWidth, availHeight }
}

const windowInfo = ref(getWindowInfo())

// Refresh info on resize event
onMounted(() => {
  window.addEventListener('resize', () => {
    windowInfo.value = getWindowInfo()
  })
})
</script>

<template>
  <header>
    <p>header</p>
  </header>

  <main>
    <p class="info" v-for="(value, key, index) in windowInfo" :key="index">
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
