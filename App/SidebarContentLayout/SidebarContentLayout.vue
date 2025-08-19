<template>
  <view class="container">
    <view class="top">
      <!-- -------- 左侧菜单 ------------ -->
      <scroll-view :scroll-y="true" class="menu">
        <view
          class="menu-item"
          v-for="item of menuList"
          :key="item"
          @click="onMenuClick(item)"
          :class="{ active: currentMenu == item }"
        >
          {{ item }}
        </view>
      </scroll-view>
      <!-- -------- 右侧内容 ------------ -->
      <scroll-view :scroll-y="true" class="content">
        <view content="" style="overflow: hidden"></view>
        <slot></slot>
        <view content="" style="overflow: hidden"></view>
      </scroll-view>
    </view>
    <!-- -------- 底部按钮 ------------ -->
    <view class="bottom">
      <button
        v-for="(item, index) of btnList"
        :key="item"
        @click="onBtnClick(item, index)"
        :class="{ active: currentBtn == item }"
      >
        {{ item }}
      </button>
    </view>
  </view>
</template>

<script setup>
/**
 * SidebarContentLayout: 左菜单 右内容 底按钮
 */
const props = defineProps({
  btnList: {
    type: Array,
    default: () => ["", ""],
  },
  menuList: {
    type: Array,
  },
});
const currentBtn = ref(props.btnList[0]);
const currentMenu = ref(props.menuList[0]);
const emit = defineEmits(["btnClick", "menuClick"]);
function onBtnClick(item, index) {
  currentBtn.value = item;
  emit("btnClick", index);
}
function onMenuClick(item) {
  currentMenu.value = item;
  emit("menuClick", item);
}
</script>

<style lang="scss" scoped>
.container {
  $bottom-height: 6vh;
  $top-height: calc(100vh - #{$bottom-height});
  --active-color: #1a66ff;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  .bottom {
    display: flex;
    width: 100%;
    height: $bottom-height;
    button {
      width: 50%;
      height: 100%;
      color: white;
      background-color: #e1effc;
      display: flex;
      justify-content: center;
      align-items: center;
      color: var(--active-color);
      border-radius: unset;

      &.active {
        background-color: var(--active-color);
        color: white;
      }
    }
  }
  .top {
    display: flex;
    .menu,
    .content {
      min-width: 0;
    }
    .menu {
      flex: 12 0 0;
      height: $top-height;
      box-sizing: border-box;
      background-color: #f7f8fa;
      display: flex;
      flex-direction: column;
      border-right: 2px solid #f7f8fa;
      $item-height: 8vh;
      .menu-item {
        &.active {
          background-color: #fff;
          color: var(--active-color);
          &::before {
            content: "";
            position: absolute;
            left: 0;
            top: 50%;
            width: 0.6vw;
            height: calc($item-height * 0.4);
            background-color: var(--active-color);
            transform: translateY(-50%);
          }
        }
        position: relative;
        display: flex;
        justify-content: left;
        align-items: center;
        width: 100%;
        height: $item-height;
        box-sizing: border-box;
        padding-left: 10%;
      }
    }
    .content {
      flex: 35 0 0;
      height: $top-height;
      background-color: white;
      padding: 0 2vw;
      box-sizing: border-box;
    }
  }
}
</style>