<script setup lang="ts">
defineProps<{
  tableData: { key: string; value: string }[]
}>()
</script>

<template>
  <!-- delimiter = table width + left margin + right margin -->
  <div mx-4 class="min-[432px]:w-400px min-[432px]:ml-auto min-[432px]:mr-auto">
    <el-table stripe border :show-header="false" :data="tableData">
      <el-table-column align="right" prop="key" />
      <el-table-column align="left" prop="value" />
    </el-table>
  </div>
</template>

<style scoped></style>
