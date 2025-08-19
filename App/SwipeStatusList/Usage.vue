<template>
  <SwipeStatusList
    ref="SRef"
    :data="data"
    @delete="onDelete"
    @itemClick="onItemClick"
    ellipsis
  ></SwipeStatusList>
</template>

<script setup>
import SwipeStatusList from "@/components/SwipeStatusList.vue";
const SRef = ref(null);
const data = ref([
  {
    id: 1,
    status: 0,
    title: "示例标题",
    content: [
      {
        label: "原神",
        value: "启动",
      },
      {
        label: "还是启动",
        value:
          "原神启动，不单是启动，而是缓启、慢启，有计划地启，在实践中边启边学习。",
      },
    ],
  },
  { id: 2, status: 1, title: "无内容" },
  { id: 3, status: 1, title: "依然无内容" },
]);
const onItemClick = (index) => {
  console.log("index", index);
};
const onDelete = (index) => {
  uni.showModal({
    title: "提示",
    content: "确定删除该用户?",
    success: function (res) {
      if (res.confirm) {
        data.value.splice(index, 1);
      }
      SRef.value.closeAll();
    },
  });
};
</script>

<style lang="scss" scoped></style>
