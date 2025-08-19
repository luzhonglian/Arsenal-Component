<template>
  <div class="no-data flex-center" :style="{ minHeight: `${minHeight}px` }">
    {{ title }}
  </div>
</template>

<script setup>
/**
 * @description:描边黑色，字体白色的无数据显示
 */
const props = defineProps({
  title: {
    type: String,
    default: "暂无数据",
  },
  minHeight: {
    type: [Number, String],
    default: 60,
  },
});
</script>

<style lang="scss" scoped>
.no-data {
  width: 100%;
  height: 100%;
  ///字体描边黑色，字体白色
  color: #fff;
  text-shadow: -1px -1px 0 #000, 1px -1px 0 #000, -1px 1px 0 #000,
    1px 1px 0 #000;
}
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
