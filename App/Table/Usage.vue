<template>
  <Table
    :tableData="tableData"
    :labels="labels"
    :fields="fields"
    :dict="dict"
  />
</template>

<script setup>
import Table from "@/components/Table.vue";
const tableData = [
  {
    name: "周杰伦",
    age: 18,
    isAdult: 1,
  },
  {
    name: "林俊杰",
    age: 16,
    isAdult: 0,
  },
];
const labels = ["姓名", "年龄", "是否成年"];
const fields = ["name", "age", "isAdult"];
/// 值对应的实际意义
const dict = {
  isAdult: {
    0: "否",
    1: "是",
  },
};
</script>

<style lang="scss" scoped></style>
