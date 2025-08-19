<template>
  <view>
    <view class="attachment" v-if="validFileType == 'all' && disabled">
      <view v-for="file of fileList" :key="file.url">
        <a :href="file.url">{{ file.name }}</a>
      </view>
    </view>

    <uni-file-picker
      :style="{ height: preview ? '50px' : '80px' }"
      v-else
      ref="FilePickerRef"
      v-model="fileList"
      :file-mediatype="validFileType"
      :image-styles="imageStyles"
      @select="select"
      :disabled="disabled"
      :readonly="disabled"
      :listStyles="listStyles"
      :file-extname="fileExtname"
      :auto-upload="false"
      @delete="onDelete"
    ></uni-file-picker>
  </view>
</template>

<script setup>
/**
 * @description 文件上传组件
 * @param {Array} fileList 文件列表
 * @param {Boolean} disabled 是否禁用
 * @param {String} uploadType 文件类型
 * @usage <Uploader v-model:fileList="fileList" fileType="img" />
 */
//` 变量配置
//#region
import { useVModel } from "@vueuse/core";
import { upload } from "@/apis/upload";
const props = defineProps({
  fileList: {
    type: Array,
    default: () => [],
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  uploadType: {
    type: String,
    default: "img", /// attachment|img 保持和网页端一致写法
  },
  preview: {
    type: Boolean,
    default: false,
  },
});
const fileTypeMap = {
  attachment: "all",
  img: "image",
};
const validFileType = fileTypeMap[props.uploadType]; /// 实际的有效值
const emit = defineEmits(["update:fileList"]);
const fileList = useVModel(props, "fileList", emit);
const FilePickerRef = ref(null);
const fileExtname = computed(() => {
  const attachment = ["pdf", "doc", "docx", "xls", "xlsx", "dwg"];
  const image = ["png", "jpg", "jpeg", "bmp"];
  return validFileType == "image" ? image : attachment;
});
//#endregion

const select = ({ tempFiles }) => {
  tempFiles.forEach((item) => {
    const { file } = item;
    uploadFile(file);
  });
};

function onDelete({ index }) {
  fileList.value.splice(index, 1);
}

//` 上传文件
//#region
const baseUrl = "";
async function uploadFile(file) {
  const formData = new FormData();
  formData.append("file", file);
  const { msg, data } = await upload(formData).then((res) => res.data);
  if (msg == "success") {
    //. 将url属性置入fileList数组的最后一个对象
    data.url = data.url.startsWith("http") ? data.url : baseUrl + data.url;
    fileList.value.push(data);
    console.log("fileList.value", fileList.value);
  }
}
//#endregion

//` 样式设置
//#region
const listStyles = {
  borderStyle: {
    color: "red", // 边框颜色
    width: "1px", // 边框宽度
    style: "solid", // 边框样式
    radius: "5px", // 边框圆角，不支持百分比
  },
  border: true, // 是否显示边框
  dividline: true, // 是否显示分隔线
};

const imageStyles = {
  height: props.preview ? 50 : 80, // 边框高度
  aspectRatio: 1,
  border: {
    // 如果为 Boolean 值，可以控制边框显示与否
    color: "#eee", // 边框颜色
    width: "1px", // 边框宽度
    style: "solid", // 边框样式
  },
};
//#endregion
</script>

<style lang="scss" scoped>
:deep() {
  .uni-file-picker__container {
    margin: unset;
  }
  .file-picker__box-content {
    margin: 2px;
  }
}
</style>
