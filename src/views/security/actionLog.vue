<template>
  <div class="post-container" v-loading="loading">

    <el-table
        :data="tableData"
        style="width: 100%;height: calc(100vh - 142px);overflow-y:scroll"
        class="elTable">
      <el-table-column label="操作时间" prop="operateTime"></el-table-column>
      <el-table-column label="设备IP" prop="ipAddress"></el-table-column>
      <el-table-column label="设备MAC" prop="macAddress"></el-table-column>
      <el-table-column label="操作行为" prop="operateAction"></el-table-column>
      <el-table-column label="操作内容" prop="operateDetail"></el-table-column>
    </el-table>
    <div class="pagination">
      <el-pagination
          small
          layout="prev, pager, next, total"
          :total="pageNum"
          @current-change="handleCurrentChange"
          :current-page.sync="currentPage">
      </el-pagination>
    </div>
  </div>
</template>

<script>
import { getLogList } from '@/network/security.js'
export default {
  name: "vueLC",
  data () {
    return {
      loading: false,
      tableData: [],
      pageNum: 1,
      currentPage: 1,
    }
  },
  mounted() {
    this.currentPage = 1;
    this.loading = false;
    this.getData(0);
  },
  methods:{
    getData(method) {
      if(method == 0) {
        getLogList(this.currentPage).then(res=>{
          this.pageNum = res.data.count;
          this.tableData = res.data.data;
          console.log(this.tableData);
          this.loading = false;
        }).catch(e=>{
          console.log(e);
        })
      }
    },

    handleCurrentChange() {
      this.loading = true;
      this.getData(0)
    },

  }
}
</script>

<style>
.el-main {
  padding: 10px 20px;
}
</style>

<style scoped>
.post-container {
  position: relative;
}
.pagination {
  position: absolute;
  right: 20px;
}
</style>
