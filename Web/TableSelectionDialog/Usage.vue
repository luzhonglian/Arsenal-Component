<template>
  <div>
    <el-button @click="isShow = true">打开dialog</el-button>
    <TableSelectionDialog
      ref="TRef"
      v-if="isShow"
      v-model="isShow"
      :selectedData="selectedData"
      :columns="columns"
      @getData="getData"
      @onConfirm="onConfirm"
    ></TableSelectionDialog>
  </div>
</template>

<script setup>
import { ref } from "vue";
import TableSelectionDialog from "@/components/TableSelectionDialog.vue";
const TRef = ref(null);
const isShow = ref(false);
const selectedData = ref([
  { id: 1, name: "周杰伦", qq: 123456 },
  { id: 10, name: "鲁仲连", qq: 198765 },
]);
const columns = ref([
  { prop: "name", label: "姓名" },
  { prop: "qq", label: "QQ" },
]);
//. dialog确认时把最新的值赋给选定值
function onConfirm(newData) {
  console.log("newData", newData);
  selectedData.value = newData;
}

function fetData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({
        data: {
          msg: "success",
          data: {
            list: testData,
            pageInfo: {
              total: 10,
            },
          },
        },
      });
    }, 10);
  });
}

async function getData(page) {
  const response = await fetData(page);
  const { msg, data } = response.data;
  if (msg == "success") {
    TRef.value.onDataFetched(data);
  }
}

const testData = [
  { id: 1, name: "周杰伦", qq: 123456 },
  { id: 2, name: "林俊杰", qq: 654321 },
  { id: 3, name: "陈奕迅", qq: 234567 },
  { id: 4, name: "王力宏", qq: 765432 },
  { id: 5, name: "薛之谦", qq: 345678 },
  { id: 6, name: "毛不易", qq: 876543 },
  { id: 7, name: "周兴哲", qq: 456789 },
  { id: 8, name: "李荣浩", qq: 987654 },
  { id: 9, name: "陶喆", qq: 567890 },
  { id: 10, name: "鲁仲连", qq: 198765 },
];
</script>

<style lang="scss" scoped></style>
