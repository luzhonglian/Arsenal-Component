<template>
  <image src="@/static/icons/close.png" class="close" />
</template>

<script setup>
/**
 * usage: <Close @click="isShow = false" />
 */
</script>

<style lang="scss" scoped>
.close {
  width: 4vh;
  height: 4vh;
  position: fixed;
  top: 0.5vh;
  right: 0;
  z-index: 9;
  background-color: white;
  border-radius: 50%;
}
</style>
