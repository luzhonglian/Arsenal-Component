<template>
  <TabsLayout :tabs="tabs" @tabClick="onTabClick">
    <component :is="currentComponent"></component>
  </TabsLayout>
</template>

<script setup>
import TabsLayout from "@/components/TabsLayout.vue";
const tabs = [
  {
    name: "基础信息",
  },
  {
    name: "再来一个",
  },
  {
    name: "还来一个",
  },
];
//` 动态组件切换
const compMap = {
  基础信息: "Basic",
  再来一个: "Another",
  还来一个: "More",
};
const currentComponent = shallowRef();
const onTabClick = ({ name }) => {
  const propKey = compMap[name];
  currentComponent.value = defineAsyncComponent(() =>
    import(`./components/${propKey}.vue`)
  );
};
function init() {
  onTabClick({ name: "基础信息" });
}
init();
</script>

<style lang="scss" scoped></style>
