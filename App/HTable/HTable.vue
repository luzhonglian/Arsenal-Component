<template>
  <view>
    <uni-section
      v-if="!noTitle"
      class="section"
      :title="title"
      type="line"
    ></uni-section>
    <view
      class="table"
      v-for="item in tableData"
      :key="item.label"
      :style="{ fontSize }"
    >
      <view
        class="label"
        :style="{
          flex: ratio[0],
          backgroundColor: lBg,
        }"
        >{{ item.label }}</view
      >
      <view class="value" :style="{ flex: ratio[1], backgroundColor: vBg }"
        >{{ item.map ? item.map[item.value] : item.value
        }}{{ item.unit || "" }}</view
      >
    </view>
  </view>
</template>

<script setup>
/**
 * HTable:Horizental Table
 */
const props = defineProps({
  title: {
    type: String,
  },
  /// [{label,value,map?,unit}]
  tableData: {
    type: Array,
    default: [],
  },
  ratio: {
    type: Array,
    default: () => [13, 17],
  },
  noTitle: {
    type: Boolean,
    default: false,
  },
  /// label background
  lBg: {
    type: String,
    default: "#f2f8ff",
  },
  /// value background
  vBg: {
    type: String,
    default: "white",
  },
  fontSize: {
    type: String,
    default: "0.8rem",
  },
});
</script>

<style lang="scss" scoped>
:deep(.uni-section__content-title.distraction) {
  font-weight: bold;
}
.table {
  display: flex;
  --border-width: 0.5px;
  $font-size: 0.8rem;
  width: 100%;
  padding: 0 4%;
  box-sizing: border-box;
  &:last-of-type {
    .label,
    .value {
      border-bottom: var(--border-width) solid #bfddff;
    }
  }
  .label,
  .value {
    padding-left: 3%;
    box-sizing: border-box;
    color: #1d2129;
    border-width: var(--border-width) 0 0 var(--border-width);
    border-style: solid;
    border-color: #bfddff;
    // font-size: $font-size;
    line-height: 2;
    display: flex;
    align-items: center;
  }
  .label {
    flex: 13;
    background-color: #f2f8ff;
  }
  .value {
    flex: 17;
    background-color: white;
    border-right-width: var(--border-width);
  }
}
</style>
