---
title: 基于Element-Plus二次封装表格业务组件
date: 2021-10-20 14:00
---

> 基于Element-Plus表格封装分页相关业务

```vue
### biz组件内容

<template>
  <el-table
    ref="bizTable"
    v-loading="loading"
    :data="data"
    v-bind="tableConfig.attributes"
    class="mb-4"
    v-on="tableConfig.events"
  >
    <el-table-column
      v-for="(item,index) in columns"
      :key="index"
      v-bind="item"
    >
      <template
        v-if="item.defaultSlot"
        #default="{row}"
      >
        <table-item :slot-func="item.defaultSlot(row)" />
      </template>
      <template
        v-if="item.headerSlot"
        #header="{column}"
      >
        <table-item :slot-func="item.headerSlot(column)" />
      </template>
    </el-table-column>
  </el-table>
  <el-pagination
    v-if="hasPageInfo"
    :current-page="pageInfo.page"
    :page-sizes="[10, 20, 50, 100]"
    :page-size="pageInfo.pageSize"
    layout="total, sizes, prev, pager, next, jumper"
    :total="pageInfo.total"
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
  />
</template>

<script>
import { defineComponent, reactive, toRefs } from 'vue'
import TableItem from './tableItem.vue'

export default defineComponent({
  name: 'BizTable',
  components: {
    'table-item': TableItem
  },
  props: {
    loading: {
      required: true,
      type: Boolean,
      default: () => true
    },
    tableConfig: {
      type: Object,
      required: true,
      default: () => {
        return {
          attributes: {},
          events: {}
        }
      }
    },
    data: {
      required: true,
      type: Array,
      default: () => []
    },
    columns: {
      required: true,
      type: Array,
      default: () => []
    },
    hasPageInfo: {
      required: true,
      type: Boolean,
      default: () => true
    },
    pageInfo: {
      required: true,
      type: Object,
      default: () => {
        return {
          total: 0,
          page: 1,
          pageSize: 10
        }
      }
    },
    queryEvent: {
      required: true,
      type: Function
    },
    queryParams: {
      required: true,
      type: Object,
      default: () => {
      }
    }
  },
  emits: ['pageInfo:update'],
  setup (props, { emit }) {
    const state = reactive({
      bizTable: null
    })
    const handleSizeChange = (val) => {
      emit('pageInfo:update', {
        total: props.pageInfo.total,
        page: props.pageInfo.page,
        pageSize: val
      })
      props.queryEvent({ ...props.queryParams, ...(props.hasPageInfo ? props.pageInfo : {}) })
    }
    const handleCurrentChange = (val) => {
      emit('pageInfo:update', {
        total: props.pageInfo.total,
        page: val,
        pageSize: props.pageInfo.pageSize
      })
      props.queryEvent({ ...props.queryParams, ...(props.hasPageInfo ? props.pageInfo : {}) })
    }
    const clearSelection = () => {
      state.bizTable.clearSelection()
    }
    const toggleRowSelection = (row, selected) => {
      state.bizTable.toggleRowSelection(row, selected)
    }
    const toggleAllSelection = () => {
      state.bizTable.toggleAllSelection()
    }
    const toggleRowExpansion = (row, expanded) => {
      state.bizTable.toggleRowExpansion(row, expanded)
    }
    const setCurrentRow = (row) => {
      state.bizTable.setCurrentRow(row)
    }
    const clearSort = () => {
      state.bizTable.clearSort()
    }
    const clearFilter = (columnKey) => {
      state.bizTable.clearFilter(columnKey)
    }
    const doLayout = () => {
      state.bizTable.doLayout()
    }
    const sort = (prop, order) => {
      state.bizTable.sort(prop, order)
    }
    return {
      ...toRefs(state),
      handleSizeChange,
      handleCurrentChange,
      clearSelection,
      toggleRowSelection,
      toggleAllSelection,
      toggleRowExpansion,
      setCurrentRow,
      clearSort,
      clearFilter,
      doLayout,
      sort
    }
  }
})
</script>

<style>

</style>

```

```vue
### 内部插槽

<script>
import { h, defineComponent } from 'vue'

export default defineComponent({
  name: 'TableItem',
  props: {
    slotFunc: {
      required: true,
      type: Object
    }
  },
  setup (props) {
    return () => h(props.slotFunc)
  }
})
</script>

<style scoped>

</style>

```

```vue
### example

<template>
  <biz-table
    :data="list"
    :loading="loading"
    :columns="columns"
    :table-config="tableConfig"
    :page-info="pageInfo"
    :has-page-info="true"
    :query-event="getPageList"
    :query-params="query"
  />
</template>

<script>
import { onMounted, reactive, defineComponent, toRefs, h, resolveComponent } from 'vue'
import BizTable from '../../components/biz/index.vue'

export default defineComponent({
  name: 'PagesIndex',
  components: {
    BizTable
  },
  setup () {
    const state = reactive({
      list: [{
        id: '12987122',
        name: '王小虎',
        amount1: '234',
        amount2: '3.2',
        amount3: 10
      }, {
        id: '12987123',
        name: '王小虎',
        amount1: '165',
        amount2: '4.43',
        amount3: 12
      }, {
        id: '12987124',
        name: '王小虎',
        amount1: '324',
        amount2: '1.9',
        amount3: 9
      }, {
        id: '12987125',
        name: '王小虎',
        amount1: '621',
        amount2: '2.2',
        amount3: 17
      }, {
        id: '12987126',
        name: '王小虎',
        amount1: '539',
        amount2: '4.1',
        amount3: 15
      }],
      query: {
        id: '',
        name: ''
      },
      loading: true,
      columns: [
        {
          prop: 'id',
          label: 'ID'
        },
        {
          label: '姓名',
          prop: 'name'
        },
        {
          label: '展开',
          type: 'expand'
        },
        {
          label: '多选',
          type: 'selection'
        },
        {
          label: '日期',
          prop: 'date',
          sortable: true,
          columnKey: 'date',
          filters: [{
            text: '2016-05-01',
            value: '2016-05-01'
          }, {
            text: '2016-05-02',
            value: '2016-05-02'
          }, {
            text: '2016-05-03',
            value: '2016-05-03'
          }, {
            text: '2016-05-04',
            value: '2016-05-04'
          }],
          filterMethod: (value, row, column) => {
            const property = column.property
            return row[property] === value
          }
        },
        {
          label: '地址',
          prop: 'address'
        }, {
          label: '金额',
          prop: 'amount3',
          formatter: (row) => `￥${row.amount3}`
        }, {
          label: 'tool',
          fixed: 'right',
          defaultSlot: (row) => {
            return h(resolveComponent('el-button'), {
              type: 'text',
              onClick: () => alert(row.name)
            }, ()=>'操作')
          },
          headerSlot: (row) => {
            return h(resolveComponent('el-button'), {
              type: 'text'
            }, ()=>'新增')
          }
        }],
      tableConfig: {
        attributes: {
          stripe: true,
          border: true,
          rowClassName: ({
            row,
            rowIndex
          }) => {
            if (rowIndex === 1) {
              return 'warning-row'
            } else if (rowIndex === 3) {
              return 'success-row'
            }
            return ''
          },
          height: 150,
          defaultSort: {
            prop: 'date',
            order: 'descending'
          },
          showSummary: true,
          spanMethod: ({
            row,
            column,
            rowIndex,
            columnIndex
          }) => {
            if (rowIndex % 2 === 0) {
              if (columnIndex === 0) {
                return [1, 2]
              } else if (columnIndex === 1) {
                return [0, 0]
              }
            }
          }
        },
        events: {
          select: (selection, row) => {
            // eslint-disable-next-line no-console
            console.log(selection)
          },
          cellClick: (row, column, cell, event) => {
            // eslint-disable-next-line no-console
            console.log(row)
          }
        }
      },
      pageInfo: {
        total: 0,
        page: 1,
        pageSize: 10
      }
    })
    const getPageList = async (params) => {
      setTimeout(() => {
        state.loading = false
        state.pageInfo.total = Math.floor(Math.random() * 100)
      }, 1000)
    }
    onMounted(() => {
      getPageList()
    })
    return {
      ...toRefs(state),
      getPageList
    }
  }
})
</script>

<style>
</style>


```
