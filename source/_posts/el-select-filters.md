---
title: 基于 element-plus 下拉搜索框大量数据的显示优化
date: 2022-01-11 14:00
---
在中后台管理系统当中，存在一种应用场景，就是多个筛选框存在大量数据，比如人员下拉，部门下来，在后端不分页的情况下，需要前端进行处理，因此对这个业务组件进行了封装。

```
<template>
  <el-select v-bind="$attrs" :filter-method="handleSearch" filterable>
    <el-option
      v-for="item in showOptions"
      :key="item[props.value]"
      :label="item[props.label]"
      :value="item[props.value]"
    >
      <template v-if="description">
        <span style="float: left">{{ item[props.label] }}</span>
        <span
          style="
            float: right;
            color: var(--el-text-color-secondary);
            font-size: 13px;
          "
        >
          {{ item[description] }}
        </span>
      </template>
    </el-option>
  </el-select>
</template>

<script>
  import { reactive, computed, toRefs } from 'vue'
  export default {
    name: 'ElSelectLazy',
    props: {
      //选项列表
      options: {
        required: true,
        type: Array,
        default: () => [],
      },
      //键值对映射
      props: {
        required: false,
        type: Object,
        default: () => {
          return {
            label: 'label',
            value: 'value',
          }
        },
      },
      //描述信息
      description: {
        required: false,
        type: String,
        default: '',
      },
    },
    setup(props, { attrs }) {
      const state = reactive({
        query: '',
        selectedModel: computed(() => {
          let value = []
          if (Array.isArray(attrs.modelValue)) {
            value = attrs.modelValue.slice()
          } else if (attrs.modelValue) {
            value = [attrs.modelValue]
          }
          return value
        }),
        selectedOptions: computed(() => {
          return props.options.filter((item) => {
            return state.selectedModel.includes(item[props.props.value])
          })
        }),
        searchOptions: computed(() => {
          const result = props.options.filter((item) => {
            return (
              item[props.props.label].includes(state.query) &&
              !state.selectedModel.includes(item[props.props.value])
            )
          })
          return result.slice(0, 10)
        }),
        showOptions: computed(() => {
          return [...state.selectedOptions, ...state.searchOptions]
        }),
      })
      const handleSearch = (query) => {
        if (!query) return
        state.query = query
      }
      return {
        ...toRefs(state),
        handleSearch,
      }
    },
  }
</script>

<style></style>

```
