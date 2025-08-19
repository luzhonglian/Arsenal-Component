<template>
  <uni-forms
    label-position="left"
    :model="formData"
    :rules="rules"
    ref="FormRef"
    :border="true"
    class="form"
  >
    <template v-for="item of itemList" :key="item.prop">
      <uni-forms-item :label="item.label" :prop="item.prop">
        <!-- -------- plain ------------ -->
        <view v-if="!item.type"> {{ formData[item.prop] }}</view>
        <!-- -------- input ------------ -->
        <uni-easyinput
          v-if="item.type == 'input'"
          v-model="formData[item.prop]"
          placeholder="请输入"
        ></uni-easyinput>
        <!-- -------- select ------------ -->
        <uni-data-select
          :placement="placement"
          v-if="item.type == 'select'"
          v-model="formData[item.prop]"
          :localdata="item.localdata"
          placeholder="请选择"
        ></uni-data-select>
        <!-- -------- date ------------ -->
        <uni-datetime-picker
          v-if="item.type == 'date'"
          v-model="formData[item.prop]"
          :type="item.dateType || 'datetime'"
          placeholder="请选择"
        ></uni-datetime-picker>
        <!-- -------- img ------------ -->
        <Gallery v-if="item.type == 'img'" :urls="formData[item.prop]" />
      </uni-forms-item>
    </template>
  </uni-forms>
</template>

<script setup>
import Gallery from "@/components/Gallery.vue";
const props = defineProps({
  ///验证规则
  rules: {
    type: Array,
  },
  ///表单项数组 [{label,prop,type,localdata,dateType?}]
  itemList: {
    type: Array,
  },
  /// 初始表单数据
  formData: {
    type: Object,
    default: () => ({}),
  },
  /// 需要watch的prop
  prop2Watch: {
    type: String,
  },
  /// 选择框出现位置
  placement: {
    type: String,
    default: "bottom",
  },
});
const formData = reactive({ ...props.formData });
const FormRef = ref(null);
//. 提交当前表单数据
async function submit() {
  try {
    await FormRef.value.validate();
    return formData;
  } catch (err) {
    console.log("表单错误信息：", err);
  }
}

defineExpose({
  submit,
});
//` 监听prop2Watch变化,用于处理选择联动
//#region
const emit = defineEmits(["propChange"]);
if (props.prop2Watch) {
  watch(
    () => formData[props.prop2Watch],
    (val) => {
      val && emit("propChange", val);
    }
  );
}
//#endregion
</script>

<style lang="scss" scoped>
.form {
  width: 100%;
  padding: 0 2vw 0 4vw;
  box-sizing: border-box;
  border-radius: 2vw;
}
:deep() {
  .uni-forms-item__label {
    width: 30% !important;
    font-weight: bold;
    color: #3b3d3d;
  }
  .uni-forms-item__content {
    width: 70% !important;
    display: flex;
    align-items: center !important;
    flex-direction: row !important;
    color: #444444 !important;
    white-space: pre-wrap;
  }
  .uni-easyinput__content,
  .uni-date-x {
    background-color: transparent !important;
  }
  .uni-date__x-input {
    font-size: 12px;
  }
  .uni-select__selector {
    z-index: 9;
  }
  .uni-calendar {
    z-index: 999;
    position: relative;
  }
}
</style>
