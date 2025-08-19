<template>
  <view class="imgs">
    <image
      mode="aspectFit"
      v-for="(url, index) in urls"
      :key="url"
      :src="url"
      class="one-img"
      @click="onImgClick(index)"
    />
  </view>
</template>

<script setup>
const props = defineProps({
  /// 图片链接
  urls: {
    type: [String, Array],
    default: [],
  },
});
const urls = ref(
  Array.isArray(props.urls) ? props.urls : props.urls.split(";").filter(Boolean)
);
function onImgClick(index) {
  uni.previewImage({
    urls: urls.value,
    current: index,
  });
}
</script>

<style lang="scss" scoped>
.imgs {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  gap: 2%;
  .one-img {
    display: block;
    border: 1px solid #e0e0e0;
    box-sizing: border-box;
    width: 32%;
    height: auto;
    aspect-ratio: 1;
  }
}
</style>
