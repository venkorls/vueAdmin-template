<template>
  <div class="app-container calendar-list-container">
    <div class="filter-container">
        <el-input @keyup.enter.native="handleFilter" style="width: 200px;" class="filter-item" placeholder="标题" v-model="listQuery.title">
      </el-input>
      <el-button class="filter-item" type="primary" v-waves icon="el-icon-search" @click="handleFilter">搜索</el-button>
      <el-button class="filter-item" style="margin-left: 10px;"  type="primary" icon="el-icon-edit">添加</el-button>
    </div>
  <el-table :key="tableKey" data="list" v-loading="listLoading" element-loading-text="加载中...." border fit highlight-current-row style="width: 100%">
<el-table-column align="center" label="序号" width="65" type="index">
</el-table-column>
<el-table-column align="center" label="名称" prop="Title" width="65">
</el-table-column>
  </el-table>
  <div v-show="!listLoading" class="pagination-container">
      <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page.sync="listQuery.page"
        :page-sizes="[10,20,30, 50]" :page-size="listQuery.limit" layout="total, sizes, prev, pager, next, jumper" :total="total">
          </el-pagination>
    </div>
  </div>
</template>

<script>
import { fetchList } from '@/api/organize'
export default {
  name: 'organize',
  data() {
    return {
      tableKey: 0,
      list: null,
      total: null,
      listLoading: true,
      listQuery: {
        page: 1,
        rows: 20,
        sord: 'desc',
        sidx: 'F_Id asc',
        keyword: 'Notice'
      }
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.listLoading = true
      fetchList(this.listQuery).then(response => {
        this.list = response.data.rows
        this.total = response.data.records
        this.listLoading = false
      })
    },
    handleFilter() {
      this.listQuery.page = 1
      this.getList()
    },
    handleSizeChange(val) {
      this.listQuery.limit = val
      this.getList()
    },
    handleCurrentChange(val) {
      this.listQuery.page = val
      this.getList()
    }
  }
}
</script>
