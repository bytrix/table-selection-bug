<template>
  <div>
    <h1>You have selected {{selectedData.length}} items.</h1>
    <el-table @select="onSelect" ref="tableRef" :data="tableData" style="width: 100%">
      <el-table-column type="selection"></el-table-column>
      <el-table-column prop="id" label="ID" width="60px" />
      <el-table-column prop="login" label="Username" width="120px" />
      <el-table-column prop="url" label="Github" />
    </el-table>
    <el-pagination
      :total="rows"
      @current-change="onPageChange"
    ></el-pagination>
  </div>
</template>

<script>
import axios from 'axios'
import { nextTick } from '@vue/runtime-core'
export default {
  data() {
    return {
      rows: 0,
      selectedData: [],
      tableData: [],
    }
  },
  mounted() {
    this.getData(1)
  },
  methods: {
    getData(page) {
      return axios.get('https://www.fastmock.site/mock/25671bee3420beb504088201c7f40a38/test/api/user?page=' + page)
        .then(({ data }) => {
          this.tableData = data.data
          this.rows = data.rows
          nextTick(() => {
            this.tableData.forEach(row => {
              if(this.selectedData.find(item => item.id === row.id)) {
                this.$refs.tableRef.toggleRowSelection(row)
              }
            })
          })
        })
    },
    onPageChange(page) {
      this.getData(page)
    },
    onSelect(selection) {
      this.selectedData = selection
    }
  }
}
</script>
