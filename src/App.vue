<script setup lang="ts">
import { computed, defineComponent, onMounted, ref } from 'vue'
import { ElConfigProvider } from 'element-plus'

defineComponent({
  components: {
    ElConfigProvider,
  },
})

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
  <el-config-provider>
    <header>
      <el-button>Default</el-button>
      <p>header</p>
    </header>

    <main>
      <p>{{ computedWindowInfo }}</p>
    </main>

    <footer>
      <!-- Create vertical scrollbar -->
      <p>footer start</p>
      <div class="scrollbar-y"></div>
      <p>footer end</p>
    </footer>
  </el-config-provider>
</template>

<style scoped>
.info {
  color: #af3a03;
}

.scrollbar-y {
  height: 1500px;
}
</style>
