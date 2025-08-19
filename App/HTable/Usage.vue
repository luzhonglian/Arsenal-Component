<template>
  <HorizentalTable title="横向表格" :tableData="tableData"></HorizentalTable>
</template>

<script setup>
import HorizentalTable from "@/components/HTable.vue";
/// [{label,value,map?,unit}]
const tableData = ref([
  { label: "姓名", value: "周杰伦" },
  { label: "专辑数量", value: 15, unit: "张" },

  {
    label: "会飞与否",
    value: "0",
    map: {
      0: "不会",
      1: "会",
    },
  },
]);
</script>

<style lang="scss" scoped></style>
