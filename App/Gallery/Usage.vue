<template>
  <Gallery :urls="urls" />
</template>

<script setup>
import Gallery from "@/components/Gallery.vue";
const getUrl = (url) => {
  return new URL(url, import.meta.url).href;
};
const urls = [
  getUrl("./cats/meiduan.webp"),
  getUrl("./cats/xianluo.webp"),
  getUrl("./cats/yingduan.webp"),
];
</script>

<style lang="scss" scoped></style>
