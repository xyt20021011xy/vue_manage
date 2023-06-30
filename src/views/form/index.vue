<template>
  <div>
    <el-table :data="tableData" style="margin-left: 75px">
      <el-table-column prop="cclass" label="班级" width="380">
        <template slot-scope="{ row }">
          {{ row.cclass + '班' }}
        </template>
      </el-table-column>
      <el-table-column prop="count" label="人数" width="380">
        <template slot-scope="{ row }">
          {{ row.count + '人' }}
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  name: 'Form',
  data() {
    return {
      tableData: []
    }
  },
  mounted() {
    console.log(sessionStorage.getItem('Authorization'))
    axios.get('http://localhost:8080/stus')
      .then(response => {
        // 处理响应数据
        console.log(response)
        this.tableData = response.data.data
      })
      .catch(error => {
        console.log(error)
      })
  }
}
</script>

<style scoped>
.el-header {
  background-color: #B3C0D1;
  color: #3a7f43;
  font-size: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 60px;
}

.sidebar {
  width: 200px;
  background-color: #d10a0a;
  padding: 10px;
}

.menu {
  background-color: transparent;
}

.el-menu-item {
  padding: 10px;
}

.el-menu-item:hover {
  background-color: #6b6868;
}

.router-link-active {
  color: #534947;
  font-weight: bold;
}
</style>
