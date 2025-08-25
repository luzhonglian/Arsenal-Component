<template>
  <el-table :data="tableData" :stripe="isStriped" :height="height">
    <el-table-column type="index" label="序号" width="60" fixed="left" />
    <el-table-column
      v-for="item in columns"
      :key="item.prop"
      :prop="item.prop"
      :label="item.label + (item.unit ? `(${item.unit})` : '')"
      :width="item.width"
    >
      <!-- -------- 展示映射 ------------ -->
      <template v-if="item.map" #default="{ row }">
        {{ item.map[row[item.prop]] }}
      </template>
      <!-- -------- 展示图片 ------------ -->
      <template v-if="item.type" #default="{ row, $index }">
        <!-- 图片展示 -->
        <template v-if="item.type === 'pic'">
          <template v-if="Array.isArray(row[item.prop])">
            <div class="imgs">
              <img
                v-for="src in row[item.prop]"
                :key="src"
                :src="src"
                alt=""
                style="margin-right: 5px"
              />
            </div>
          </template>
          <img v-else :src="row[item.prop]" alt="" />
        </template>
        <!-- 时间选择器 / 下拉框 / 输入框 -->
        <template v-else-if="item.type !== 'upload'">
          <FormField
            :type="item.type"
            :options="item.options"
            :dateType="item.dateType"
            :placeholder="item.placeholder"
            :disabled="item.disabled"
            :clearable="false"
            :filterable="false"
            teleported
            v-model="item.list[$index][item.prop]"
          />
        </template>
        <!-- 上传 -->
        <template v-else>
          <Uploader
            style="display: flex; align-items: center"
            v-model:fileList="item.list[$index][item.prop]"
            :type="item.uploadType"
            :disabled="item.disabled"
          />
        </template>
      </template>
    </el-table-column>
    <!-- -------- row的操作 ------------ -->
    <el-table-column
      v-if="!noAction"
      label="操作"
      fixed="right"
      :width="actionCnt > 1 ? actionCnt * 40 : 60"
    >
      <template #default="{ row }">
        <div
          class="row-action"
          :style="{
            justifyContent: actionCnt > 1 ? 'space-around' : 'center',
          }"
        >
          <el-icon
            v-if="!noDetail"
            :size="iconSize"
            color="#409efc"
            title="详情"
            @click="onClick('detail', row)"
          >
            <Document />
          </el-icon>
          <el-icon
            v-if="!noEdit"
            :size="iconSize"
            color="#409efc"
            title="编辑"
            @click="onClick('edit', row)"
          >
            <Edit />
          </el-icon>
          <el-icon
            v-if="!noDelete"
            :size="iconSize"
            color="#409efc"
            title="删除"
            @click="onClick('delete', row)"
          >
            <Delete />
          </el-icon>
          <el-icon
            v-if="locate"
            :size="iconSize"
            color="#409efc"
            title="定位"
            @click="onClick('locate', row)"
          >
            <Location />
          </el-icon>
        </div>
      </template>
    </el-table-column>
  </el-table>
</template>

<script setup>
/**
 * 表格组件
 *
 * @description 用于展示数据表格，支持图片、文件上传、下拉框、输入框、日期选择等多种列类型
 *
 * @param {Array<Object>} columns 表格列配置
 * @param {string} columns[].label 列名
 * @param {string} columns[].prop 对应字段
 * @param {number} [columns[].width] 列宽
 * @param {Object} [columns[].map] 值映射
 * @param {"pic"|"upload"|"select"|"input"|"picker"} [columns[].type] 列类型
 * @param {string} [columns[].dateType] 日期类型 (仅当 type="picker" 时生效)
 * @param {Array} [columns[].options] 下拉框选项 (仅当 type="select" 时生效)
 *
 * @param {Array<Object>} tableData 表格数据
 * 每项包含：
 *   { type: "pic" | "upload" | "select" | "input" | "picker", ... }
 *
 * @param {number} [iconSize=16] 图标大小
 * @param {boolean} [noDetail=false] 是否隐藏详情按钮
 * @param {boolean} [noEdit=false] 是否隐藏编辑按钮
 * @param {boolean} [noDelete=false] 是否隐藏删除按钮
 * @param {boolean} [noAction=false] 是否隐藏操作列
 *
 * @usage
 * <Table :columns="columns" :tableData="tableData" @rowAction="onRowAction" />
 */

import Uploader from "@/components/Uploader.vue";
const props = defineProps({
  columns: {
    type: Array,
    default: () => [],
  },
  tableData: {
    type: Array,
    default: () => [],
  },
  iconSize: {
    type: Number,
    default: 22,
  },
  noDetail: {
    type: Boolean,
    default: false,
  },
  noEdit: {
    type: Boolean,
    default: false,
  },
  noDelete: {
    type: Boolean,
    default: false,
  },
  noAction: {
    type: Boolean,
    default: false,
  },
  isStriped: {
    type: Boolean,
    default: true,
  },
  locate: {
    type: Boolean,
  },
  height: {
    type: [Number, String],
  },
});
//` 判断操作是否只有一个图标
const actionCnt = computed(() => {
  return (
    (props.noDetail === false) +
    (props.noEdit === false) +
    (props.noDelete === false) +
    (props.locate === true)
  );
});
//` 设置表头的操作文字居中

const emit = defineEmits(["rowAction"]);
const onClick = (type, row) => {
  emit("rowAction", { type, row });
};
</script>

<style lang="scss" scoped>
.el-table {
  flex: 1;
  margin-bottom: 5px;
}
.row-action {
  display: flex;
  align-items: center;
  .el-icon {
    cursor: pointer;
  }
}
</style>
