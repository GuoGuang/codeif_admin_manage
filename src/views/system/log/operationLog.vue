<template>
  <!-- 操作日志列表 -->
  <div class="app-container">
    <el-header style="padding:0 0 0 0px;">
      <div class="filter-container">
        <el-input v-model="listQuery.clientIp" prefix-icon="el-icon-search" style="width: 150px;" class="filter-item" placeholder="ip段" clearable @keyup.enter.native="getRightList" />
        <!--  @click="getRightList" -->
        <el-button class="filter-item" type="primary" icon="el-icon-search" plain @click="getList">
          搜索
        </el-button>
      </div>
    </el-header>

    <el-table
      ref="multipleTable"
      v-loading="listLoading"
      :data="list"
      border
      fit
      highlight-current-row
      style="width: 100%"
      @selection-change="changeFun"
      @row-click="handleCurrentRowClick"
    >
      <el-table-column prop="id" label="id" align="center" type="selection" />
      <el-table-column prop="userName" label="用户" align="center" />
      <el-table-column prop="clientIp" label="ip地址" align="center" />
      <el-table-column prop="browser" label="浏览器" align="center" />
      <el-table-column prop="osInfo" label="操作系统" align="center" />
      <el-table-column prop="type" label="操作类型" align="center" width="100">
        <template slot-scope="scope">
          <div v-for="logState in $store.getters.logState" :key="logState.value">
            <el-tag
              v-if="scope.row.type == logState.value"
              :key="logState.value"
              :type="logState.type"
            >
              {{ logState.label }}
            </el-tag>
          </div>
        </template>
      </el-table-column>
      <el-table-column :show-overflow-tooltip="true" prop="method" label="操作函数名" align="center" />
      <!-- <el-table-column prop="params" label="参数" align="center" /> -->
      <el-table-column :formatter="common.dateFormat" prop="createAt" label="操作时间" align="center" />
    </el-table>
    <div class="pagination-container">
      <el-pagination
        :current-page.sync="listQuery.page"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="listQuery.size"
        :total="total"
        layout="total, sizes, prev, pager, next, jumper"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>

<script>

import { fetchOptLogList } from '@/api/system/operationLog'
// import Pagination from '@/components/Pagination' // Secondary package based on el-pagination

export default {
  name: 'OperationLog',
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },

  data() {
    return {
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        clientIp: '',
        page: 1,
        size: 10
      },
      // 数据状态下拉选择
      dataState: this.$store.getters.dataState,
      // 选中的行
      multipleSelection: []

    }
  },
  // 注册组件
  // components: { Pagination },

  created() {
    this.getList()
  },

  methods: {

    /**
     * 查询日志列表
     */
    getList() {
      this.listLoading = true
      const data = Object.assign({}, this.listQuery, {
        page: this.listQuery.page - 1
      })
      fetchOptLogList(data).then(response => {
        if (response.data) {
          this.list = response.data.content
          this.total = response.data.totalElements
        }
        this.listLoading = false
      })
    },

    // size变更事件
    handleSizeChange(val) {
      this.listQuery.size = val
      this.listQuery.page = 1
      this.getList()
    },
    // 当前页变更事件
    handleCurrentChange(val) {
      this.listQuery.page = val
      this.getList()
    },

    /**
     * 复选框change事件
     */
    changeFun(selection) {
      this.multipleSelection = selection
    },
    /**
     * 点击当前行任意地方选中当前行
     */
    handleCurrentRowClick(selection) {
      this.$refs.multipleTable.toggleRowSelection(selection)
    }
  }
}
</script>

