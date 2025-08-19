<template>
  <div style="width: 100%">
    <el-upload
      ref="UploadRef"
      v-model:file-list="fileList"
      action=""
      :on-success="onSuccess"
      :limit="single ? 1 : 10"
      :on-exceed="onExceed"
      :http-request="httpRequest"
      :before-upload="beforeUpload"
      :list-type="type == 'img' ? 'picture-card' : 'text'"
      :on-preview="onPicPreview"
      :on-remove="onPicRemove"
      :disabled="disabled"
      :class="{ isDisabled: disabled }"
    >
      <template #default v-if="!disabled">
        <el-icon :class="{ 'avatar-uploader-icon': type == 'att' }"
          ><Plus
        /></el-icon>
      </template>
    </el-upload>

    <el-dialog
      class="preview-dialog"
      v-model="dialogVisible"
      :append-to-body="true"
      top="50px"
    >
      <img :src="dialogImageUrl" alt="图片预览" />
    </el-dialog>
  </div>
</template>

<script setup>
/**
 * @description 上传组件,上传图片或附件
 * @description 图片：上传前是上传icon，上传后是图片缩略预览
 * @description 附件：支持pdf、word、Excel
 * @usage <Uploader v-model:fileList="formData.fileList" type="att" />
 */
const props = defineProps({
  modelValue: {
    type: String,
  },
  type: {
    type: String,
    default: "img", /// img | att:附件attachment缩写
  },
  fileList: {
    type: Array,
    default: () => [],
  },
  single: {
    type: Boolean,
    default: false,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
});
const baseUrl = "";
const emit = defineEmits(["update:fileList"]);

const fileList = computed({
  get() {
    return props.fileList || [];
  },
  set(val) {
    emit("update:fileList", val);
  },
});
const uploaded = ref(false);
//` 网络请求
import { upload } from "@/api/upload"; /// 上传接口
async function httpRequest(instance) {
  const file = instance.file;
  file.status = "uploading";
  const formData = new FormData();
  formData.append("file", file);
  const { msg, data } = await upload(formData).then((res) => res.data);
  if (msg == "success") {
    uploaded.value = true;
    //. 将url属性置入fileList数组的最后一个对象
    const lastFile = fileList.value[fileList.value.length - 1];
    lastFile.url = data.url.startsWith("http") ? data.url : baseUrl + data.url;
  } else {
    ElMessage.error(msg);
  }
}
//` 处理重传图片
import { genFileId } from "element-plus";
const UploadRef = ref(null);
function onExceed(files) {
  UploadRef.value.clearFiles();
  const file = files[0];
  file.uid = genFileId();
  UploadRef.value.handleStart(file);
  UploadRef.value.submit();
}
//` 处理上传前文件格式判断
function beforeUpload(file) {
  let permit = true;
  if (props.type == "img") {
    permit = /image\/.*/.test(file.type);
  }
  if (props.type == "att") {
    permit = /\.(pdf|doc|docx|xls|xlsx|dwg)$/.test(file.name);
  }
  if (!permit) {
    ElMessage.error(
      props.type == "img" ? "请上传图片" : "请上传pdf、word、Excel、dwg格式文件"
    );
  }
  return permit;
}
//` 处理上传成功
function onSuccess(response, file, fileList) {}
//` 处理图片预览
const dialogVisible = ref(false);
const dialogImageUrl = ref("");
function onPicPreview(file) {
  if (props.type == "img") {
    dialogImageUrl.value = file.url;
    dialogVisible.value = true;
  }
  if (props.type == "att" && props.disabled) {
    window.open(file.url);
  }
}
//` 处理图片删除
function onPicRemove(file, fileList) {}
</script>

<style lang="scss" scoped>
:deep() {
  .el-upload.el-upload--picture-card {
    --el-upload-picture-card-size: 80px;
    background-color: white;
  }
  .el-upload-list--picture-card {
    .el-upload-list__item {
      width: 80px !important;
      height: 80px !important;
    }
  }
}

.isDisabled {
  display: flex;
  flex-direction: column;
  :deep() {
    .el-upload--picture-card {
      display: none;
    }
    .el-upload-list__item.is-success {
      label {
        display: none;
      }
    }
    .el-upload-list__item-file-name {
      background-color: transparent;
    }
  }
}

.el-icon.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 80px;
  height: 80px;
  box-sizing: border-box;
  text-align: center;
  border: 1px solid #dcdfe6;
}
</style>
