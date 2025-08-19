<template>
  <uni-table class="table" border :stripe="stripe" emptyText="暂无更多数据">
    <uni-tr>
      <uni-th width="1px" v-for="label of labels" :key="label" align="left">{{
        label
      }}</uni-th>
    </uni-tr>
    <!-- -------- td ------------ -->
    <uni-tr v-for="(item, index) of tableData" :key="index">
      <uni-td v-for="field of fields" :key="field" align="left">{{
        dict?.[field] ? dict[field][item[field]] : item[field]
      }}</uni-td>
    </uni-tr>
  </uni-table>
</template>

<script setup>
const props = defineProps({
  /// 表头的文字说明
  labels: {
    type: Array,
    default: () => [],
  },
  /// 表头的文字说明对应的字段
  fields: {
    type: Array,
    default: () => [],
  },
  /// 表格数据
  tableData: {
    type: Array,
    default: () => [],
  },
  /// dictionary:字段实际的值对应的文字
  dict: {
    type: Object,
    default: () => {},
  },
  /// 是否显示斑马线样式
  stripe: {
    type: Boolean,
    default: false,
  },
});
</script>

<style lang="scss" scoped>
.table {
  margin: 2vh 0;
  padding: 0 1%;
  box-sizing: border-box;
  --border-color: #bfddff;
  --th-color: #d9eaff;
  $border-style: 0.8px solid var(--border-color);
  :deep() {
    th {
      background-color: var(--th-color);
      color: #1d2129;
      font-weight: 400;
    }
    table {
      border: $border-style;
      border-bottom: unset;
      tr {
        th,
        td {
          font-size: 0.8rem;
          border-bottom: $border-style;
          &:not(:last-child) {
            border-right: $border-style;
          }
        }
      }
    }
  }
}
</style>
