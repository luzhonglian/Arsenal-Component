<template>
  <view>
    <Form
      :rules="rules"
      :itemList="itemList"
      :formData="formData"
      ref="FRef"
    ></Form>
    <button @click="onClick">提交</button>
  </view>
</template>

<script setup>
import Form from "@/components/Form.vue";
const FRef = ref(null);
async function onClick() {
  /// 最新的formData定义在Form组件，取出进行后续处理
  const newData = await FRef.value.submit();
  console.log("newData", newData);
}
const rules = [];
//` 获取初始formData
const formData = reactive({
  content: "内容",
});
///[{label,prop,type,localdata,dateType?}]
const itemList = ref([
  { label: "纯内容展示", prop: "content" },
  { label: "用户名", prop: "uName", type: "input" },
  {
    label: "用户组",
    prop: "uGroup",
    type: "select",
    localdata: [
      { text: "管理员", value: "admin" },
      { text: "普通用户", value: "user" },
    ],
  },
  {
    label: "日期选择",
    prop: "date",
    type: "date",
    dateType: "date",
  },
  {
    label: "日期范围选择",
    prop: "daterange",
    type: "date",
    dateType: "daterange",
  },
  {
    label: "时间选择",
    prop: "datetime",
    type: "date",
    dateType: "datetime",
  },
  {
    label: "时间范围选择",
    prop: "datetimerange",
    type: "date",
    dateType: "datetimerange",
  },
]);
</script>

<style lang="scss" scoped></style>
