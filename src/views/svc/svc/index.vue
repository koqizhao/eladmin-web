<template>
  <div class="service-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <el-input v-model="query.name" clearable placeholder="输入名称搜索" style="width: 200px" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <date-range-picker v-model="query.createTime" class="date-item" />
        <rrOperation />
      </div>
    </div>
    <!--表格渲染-->
    <el-table ref="table" v-loading="crud.loading" :data="crud.data" highlight-current-row style="width: 100%" @selection-change="crud.selectionChangeHandler" @current-change="handleCurrentChange">
      <el-table-column type="selection" width="55" />
      <el-table-column prop="name" label="服务名称" />
      <el-table-column prop="department" label="部门" />
      <el-table-column prop="url" label="地址" />
    </el-table>
    <!--分页组件-->
    <pagination />
  </div>
</template>

<script>
import CRUD, { presenter, header, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import pagination from '@crud/Pagination'
import DateRangePicker from '@/components/DateRangePicker'

export default {
  name: 'Service',
  components: { pagination, rrOperation, DateRangePicker },
  cruds() {
    return CRUD({ title: '服务', url: 'api/svc', crudMethod: { }})
  },
  mixins: [presenter(), header(), crud()],
  data() {
    return {
      currentRow: null,
      permission: {
      },
      rules: {
        name: [
          { required: true, message: '请输入服务名称', trigger: 'blur' }
        ],
        department: [
          { required: true, message: '请输入服务部门', trigger: 'blur', type: 'number' }
        ],
        url: [
          { required: true, message: '请输入服务地址', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    handleCurrentChange(row) {
      this.currentRow = JSON.parse(JSON.stringify(row))
    }
  }
}
</script>

<style scoped>
</style>
