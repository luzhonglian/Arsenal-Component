<template>
  <uni-swipe-action ref="ActionRef">
    <uni-swipe-action-item
      :right-options="options"
      @click="onClick(index)"
      v-for="(item, index) in data"
      :key="item[itemKey]"
      @change=""
    >
      <view class="info-item" @click="onItemClick(index)">
        <uni-section
          class="section"
          :title="item[titleProp]"
          type="line"
          titleFontSize="1rem"
          padding="0"
        >
          <template v-slot:right v-if="!noStatus">
            <view
              class="status"
              :style="{
                background: item[statusProp] == abCode ? abBg : bg,
              }"
              v-if="item[statusProp] != undefined"
            >
              <view class="left-semicircle"></view>
              <view class="content">{{
                statusLabels[Number(item[statusProp])]
              }}</view>
              <view class="right-semicircle"></view>
            </view>
          </template>
        </uni-section>
        <view class="row" v-for="row in item[contentProp]" :key="row.label">
          <view class="left">{{ row.label }}</view>
          <view class="right" :class="{ ellipsis: ellipsis }">{{
            row.map ? row.map[row.value] : row.value
          }}</view>
        </view>
      </view>
    </uni-swipe-action-item>
  </uni-swipe-action>
</template>

<script setup>
/**
 * 可滑动以删除的带状态展示的列表
 */
const ActionRef = ref(null);
const props = defineProps({
  ///数据数组[{}]，每一个数据有:标题、状态、内容
  data: {
    type: Array,
    default: () => [],
  },
  ///每一个数据的唯一标识
  itemKey: {
    type: String,
    default: "id",
  },
  ///左侧标题的属性字段,一般都是title
  titleProp: {
    type: String,
    default: "title",
  },
  //. 状态相关
  //#region
  ///状态标签数组
  statusLabels: {
    type: Array,
    default: () => ["异常", "正常"],
  },
  ///状态属性字段
  statusProp: {
    type: String,
    default: "status",
  },
  ///无需状态展示
  noStatus: {
    type: Boolean,
    default: false,
  },
  ///正常背景色
  bg: {
    type: String,
    default: "#00b42a",
  },
  ///异常背景色:abnormal background
  abBg: {
    type: String,
    default: "#f53f3f",
  },
  ///异常状态值 abnormal code
  abCode: {
    type: [Number, String],
    default: 0,
  },
  //#endregion
  //. 内容相关
  //#region
  /// 右侧换行内容是否省略显示
  ellipsis: {
    type: Boolean,
    default: false,
  },
  /// 每一个数据的内容
  contentProp: {
    type: String,
    default: "content",
  },
  //#endregion
});
/// 单数据的删除和点击
const emit = defineEmits(["delete", "itemClick"]);

//` 处理删除
const options = ref([
  {
    text: "删除",
    style: {
      backgroundColor: "#F4333C",
    },
  },
]);
const onClick = (index) => {
  console.log("index", index);
  emit("delete", index);
};
function closeAll() {
  ActionRef.value.closeAll();
}
defineExpose({
  closeAll,
});
//` 处理item点击
const onItemClick = (index) => {
  emit("itemClick", index);
};
</script>

<style lang="scss" scoped>
$radius: 2vw;
.info-item {
  letter-spacing: 0.2vw;
  background-color: #eff6fd;
  margin: 1vh 0;
  border-radius: $radius;
  border: 1px solid transparent;
  padding-bottom: 0.6vh;
  .row {
    padding: 0 4%;
    line-height: 2;
    font-size: 0.8rem;
  }
}
:deep() {
  .uni-section-header {
    border-radius: $radius $radius 0 0;
    padding: 0.8vh 0 !important;
    padding-right: 4% !important;
    background-color: #eff6fd;
    color: #3b3d3d;
    font-weight: bold !important;
    .status {
      $line-height: 1.8;
      $font-size: 0.7rem;
      $height: calc($line-height * $font-size);
      font-size: $font-size;
      line-height: $line-height;
      color: white;
      font-weight: 400 !important;
      position: relative;
      margin-right: calc($height / 2);
      padding: 0 calc($height / 2);
      border-radius: $height;
      .content {
        position: relative;
        z-index: 1;
      }
    }
  }
}
</style>
