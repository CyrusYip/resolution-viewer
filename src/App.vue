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

const tableData = [
  {
    key: 'Physical resolution',
    value: computedWindowInfo.value.physicalResolution,
  },
  {
    key: 'Logical resolution',
    value: computedWindowInfo.value.logicalResolution,
  },
  {
    key: 'Viewport size',
    value: computedWindowInfo.value.viewportSize,
  },
  {
    key: 'Scale',
    value: computedWindowInfo.value.scale,
  },
]
</script>

<template>
  <el-config-provider>
    <header>
      <p>header</p>
    </header>

    <main>
      <el-table class="infoTable" stripe :data="tableData">
        <el-table-column align="right" prop="key" />
        <el-table-column align="left" prop="value" />
      </el-table>
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
.scrollbar-y {
  height: 1500px;
}

.infoTable {
  width: 100%;
}
</style>
