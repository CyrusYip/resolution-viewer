<script setup lang="ts">
import { computed, defineComponent, onMounted, ref } from 'vue'
import { ElConfigProvider } from 'element-plus'
import InfoTable from './components/InfoTable.vue'

defineComponent({
  components: {
    ElConfigProvider
  }
})

function getWindowInfo() {
  const { innerWidth, innerHeight, outerWidth, outerHeight, devicePixelRatio } = window
  const { width, height, availWidth, availHeight } = window.screen
  return {
    innerWidth,
    innerHeight,
    outerWidth,
    outerHeight,
    devicePixelRatio,
    width,
    height,
    availWidth,
    availHeight
  }
}

const rawWindowInfo = ref(getWindowInfo())

const computedWindowInfo = computed(() => {
  const {
    innerWidth,
    innerHeight,
    width: logicalWidth,
    height: logicalHeight,
    devicePixelRatio
  } = rawWindowInfo.value
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

const tableData = computed(() => {
  return [
    {
      key: 'Physical resolution',
      value: computedWindowInfo.value.physicalResolution
    },
    {
      key: 'Logical resolution',
      value: computedWindowInfo.value.logicalResolution
    },
    {
      key: 'Viewport size',
      value: computedWindowInfo.value.viewportSize
    },
    {
      key: 'Scale',
      value: computedWindowInfo.value.scale
    }
  ]
})
</script>

<template>
  <el-config-provider>
    <div flex flex-col justify-between min-h-vh min-h-dvh>
      <header></header>
      <main>
        <InfoTable :tableData="tableData" />
      </main>
      <footer text-center>
        <p>
          <el-link
            type="primary"
            href="https://github.com/CyrusYip/resolution-viewer"
            target="_blank"
            title="Source code"
            >CyrusYip/resolution-viewer</el-link
          >
        </p>
      </footer>
    </div>
  </el-config-provider>
</template>

<style scoped></style>
