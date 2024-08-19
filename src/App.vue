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

const tableData = computed(() => {
  return [
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
})
</script>

<template>
  <el-config-provider>
    <header>
      <p>header</p>
    </header>

    <main>
      <div class="tableWrapper">
        <el-table class="infoTable" :data="tableData">
          <el-table-column align="right" prop="key" />
          <el-table-column align="left" prop="value" />
        </el-table>
      </div>
    </main>

    <footer>
      <p text-red>footer</p>
    </footer>
  </el-config-provider>
</template>

<style scoped>
.tableWrapper {
  margin: 0 1rem;
}

/* delimiter = table width + left margin + right margin */
@media (min-width: 432px) {
  .tableWrapper {
    width: 400px;
    margin-left: auto;
    margin-right: auto;
  }
}
</style>
