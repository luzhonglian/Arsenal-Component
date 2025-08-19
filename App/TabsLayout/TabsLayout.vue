<template>
  <view class="tabs-layout" :style="{ background: bg }">
    <up-tabs
      :list="tabs"
      @click="onClick"
      :inactiveStyle="inactiveStyle"
      :activeStyle="activeStyle"
      :itemStyle="itemStyle"
      :lineColor="lineColor"
    ></up-tabs>
    <scroll-view :scroll-y="true" class="content">
      <slot></slot>
    </scroll-view>
  </view>
</template>

<script setup>
const props = defineProps({
  /// [{name:''}]
  tabs: {
    type: Array,
    default: () => [],
  },
  activeColor: {
    type: String,
    default: "#1A66FF",
  },
  inactiveColor: {
    type: String,
    default: "#1D2129",
  },
  /// background
  bg: {
    type: String,
    default: "linear-gradient(to bottom, #e0eefc, #f7f8fa)",
  },
  lineColor: {
    type: String,
    default: "#1A66FF",
  },
});
const emit = defineEmits(["tabClick"]);
const onClick = (tab) => {
  emit("tabClick", tab);
};
const activeStyle = {
  color: props.activeColor,
  fontSize: "16px",
  fontWeight: 500,
};
const inactiveStyle = {
  color: props.inactiveColor,
  fontSize: "16px",
};
const itemStyle = {
  width: 100 / (props.tabs.length || 1) + "%",
  lineHeight: "42px",
};
</script>

<style lang="scss" scoped>
.tabs-layout {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 3;
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  .content {
    flex: 1;
    min-height: 0;
  }
}
</style>