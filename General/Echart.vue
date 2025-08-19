<template>
  <div ref="EchartRef" :style="style"></div>
</template>

<script setup>
/**
 * @description: echarts组件
 * @usage: <Echart :option="option" />
 */
import { useEventListener } from "@vueuse/core";
import * as echarts from "echarts";
let chartInstance;
const EchartRef = ref();
const props = defineProps({
  width: {
    type: [String, Number],
    default: "100%",
  },
  height: {
    type: [String, Number],
    default: "100%",
  },
  background: {
    type: String,
    default: "unset",
  },
  option: {
    type: Object,
    default: () => ({}),
  },
});
const { width, height, background, option } = toRefs(props);

const formatSize = (size) => (isNaN(+size) ? size : `${size}px`);
const style = computed(() => {
  return {
    width: formatSize(width.value),
    height: formatSize(height.value),
    background,
  };
});
nextTick(() => {
  /// 用nextTick确保dom已经渲染完成,可确保能拿到正确的父组件宽高
  chartInstance = echarts.init(EchartRef.value, null, { locale: "ZH" });
  setOption();
});

//`传入的的高宽改变会resize
watch([width, height], () => {
  myChart.resize();
});
//` 深度监听option的变化
watch(option, setOption, { deep: true });

//`窗口的高宽改变会resize
useEventListener(window, "resize", resize);

function resize() {
  chartInstance.resize();
}

function clear() {
  chartInstance.clear();
}

function setOption() {
  chartInstance.setOption(option.value || {});
}

defineExpose({
  setOption,
  clear,
  resize,
});
</script>

<style lang="scss" scoped></style>