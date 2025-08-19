<template>
  <SidebarContentLayout
    :menuList="menuList"
    :btnList="['新增', '筛选']"
    @menuClick="onMenuClick"
    style="--active-color: #3479e7"
  >
    <component :is="currentComponent" ref="CRef"></component>
  </SidebarContentLayout>
</template>

<script setup>
import SidebarContentLayout from "@/components/SidebarContentLayout.vue";
const menuList = ["美食区", "萌宠区"];
const menuMap = {
  美食区: "Food",
  萌宠区: "Pet",
};
const currentComponent = shallowRef();
const CRef = ref(null);
function onMenuClick(item) {
  const key = menuMap[item];
  currentComponent.value = defineAsyncComponent(() =>
    import(`./components/${key}.vue`)
  );
}
onLoad(() => {
  onMenuClick(menuList[0]);
});
</script>

<style lang="scss" scoped></style>
