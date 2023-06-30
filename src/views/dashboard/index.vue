<template>
  <div class="class-view">
    <el-form
      :inline="true"
      :model="formInline"
      class="demo-form-inline"
      style="margin-top: 20px; margin-left: 50px;"
    >
      <el-form-item  label="学号">
        <el-input v-model="formInline.userid" placeholder="学号" />
      </el-form-item>
      <el-form-item label="姓名">
        <el-input v-model="formInline.name" placeholder="姓名" />
      </el-form-item>
      <el-form-item label="班级">
        <el-select v-model="formInline.cclass" placeholder="班级">
          <el-option label="1班" value="1" />
          <el-option label="2班" value="2" />
          <el-option label="3班" value="3" />
          <el-option label="4班" value="4" />
          <el-option label="5班" value="5" />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button  type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
    <el-button style="margin-left: 50px" type="primary" @click="showAddDialog">添加学生信息</el-button>
    <el-dialog
      title="添加数据"
      :visible.sync="addDialogVisible"
      @close="handleAddDialogClose"
    >
      <el-form :model="newDataForm">
        <!-- 添加数据的表单内容 -->
        <el-form-item label="学号">
          <el-input v-model="newDataForm.userid" />
        </el-form-item>
        <el-form-item label="姓名">
          <el-input v-model="newDataForm.name" />
        </el-form-item>
        <el-form-item label="性别">
          <el-input v-model="newDataForm.gender" />
        </el-form-item>
        <el-form-item label="班级">
          <el-input v-model="newDataForm.cclass" />
        </el-form-item>
        <el-form-item label="入学时间">
          <el-input v-model="newDataForm.createTime" />
        </el-form-item>
        <el-form-item label="qq">
          <el-input v-model="newDataForm.qq" />
        </el-form-item>
        <el-form-item label="电话号码">
          <el-input v-model="newDataForm.phone" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancelAdd">取消</el-button>
        <el-button type="primary" @click="confirmAdd">确定</el-button>
      </div>
    </el-dialog>
    <el-table
      :data="tableData"

      style="width: 100%"
    >
      <el-table-column
        prop="userid"
        label="学号"
        width="150"
      />
      <el-table-column
        prop="name"
        label="姓名"
        width="150"
      />
      <el-table-column
        prop="gender"
        label="性别"
        width="150"
      />
      <el-table-column
        prop="cclass"
        label="班级"
        width="110"
      />
      <el-table-column
        prop="createTime"
        label="入学时间"
        width="180"
      />
      <el-table-column
        prop="qq"
        label="QQ"
        width="140"
      />
      <el-table-column
        prop="phone"
        label="电话号码"
        width="100"
      />
      <el-table-column
        label="操作"
        width="150"
      >
        <template slot-scope="scope">
          <el-button type="primary" size="mini" @click="handleEdit(scope.row)">编辑</el-button>
          <el-dialog
            title="编辑数据"
            :visible.sync="editDialogVisible"
            :modal="false"
            @close="handleEditDialogClose"
          >
            <el-form :model="updataDataForm">
              <!-- 添加数据的表单内容 -->
              <el-form-item label="学号">
                <el-input v-model="updataDataForm.userid" />
              </el-form-item>
              <el-form-item label="姓名">
                <el-input v-model="updataDataForm.name" />
              </el-form-item>
              <el-form-item label="性别">
                <el-input v-model="updataDataForm.gender" />
              </el-form-item>
              <el-form-item label="班级">
                <el-input v-model="updataDataForm.cclass" />
              </el-form-item>
              <el-form-item label="入学时间">
                <el-input v-model="updataDataForm.createTime" />
              </el-form-item>
              <el-form-item label="qq">
                <el-input v-model="updataDataForm.qq" />
              </el-form-item>
              <el-form-item label="电话号码">
                <el-input v-model="updataDataForm.phone" />
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="cancelUpdate">取消</el-button>
              <el-button type="primary" @click="confirmUpdate">确定</el-button>
            </div>
          </el-dialog>
          <el-button size="mini" type="danger" @click="showDeleteDialog(scope.row)">删除</el-button>
          <el-dialog
            title="确认删除"
            :visible.sync="dialogVisible"
            :modal="false"
            @close="handleDialogClose"
          >
            <span>确定要删除吗？</span>
            <span slot="footer" class="dialog-footer">
              <el-button @click="cancelDelete">取消</el-button>
              <el-button type="primary" @click="confirmDelete(scope.row)">确定</el-button>
            </span>
          </el-dialog>
        </template>
      </el-table-column>
    </el-table>
    <div class="page-footer">
      <div class="pagination-wrapper">
        <el-pagination
          :current-page.sync="currentPage1"
          :page-size="5"
          layout="total, prev, pager, next"
          :total="count"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        />
      </div>
    </div>
    </div>

</template>

<script>
import axios from 'axios'

export default {
  name: 'Dashboard',
  data() {
    return {
      count: 50,
      // 表格数据
      tableData: [],
      // 表头查询
      formInline: {
        userid: '',
        name: '',
        cclass: '',
        pageNum: 1,
        pageSize: 5
      },
      // 删除
      dialogVisible: false,
      selectedItem: null,
      // 添加
      addDialogVisible: false,
      updataDataForm: {
        userid: '',
        name: '',
        gender: '',
        cclass: '',
        createTime: '',
        qq: '',
        phone: ''
      },
      newDataForm: {
        userid: '',
        name: '',
        gender: '',
        cclass: '',
        createTime: '',
        qq: '',
        phone: ''

      },
      // 编辑
      editDialogVisible: false, // 编辑对话框可见性
      editedData: null, // 存储当前正在编辑的数据
      // 分页
      totalItems: 1000, // 总条目数
      pageSize: 5, // 每页显示的条目数
      pageNum: 1 // 当前页码
    }
  },

  mounted() {
    axios.get('http://localhost:8080/stuscount')
      .then(response => {
        // 处理响应数据
        this.count = response.data.data()
      })
      .catch(error => {
        console.log(error)
      })
    const requestData = {
      pageNum: 1,
      pageSize: 5
    }

    axios.post('http://localhost:8080/stus/search', requestData, {
      headers: {

        'Content-Type': 'application/json'
      }
    })
      .then(response => {
        // 处理响应数据
        this.tableData = response.data.data.rows
      })
      .catch(error => {
        console.log(error)
      })
  },
  methods: {
    // 编辑
    handleEdit(row) {
      // 处理编辑操作
      this.editDialogVisible = true
      this.updataDataForm = Object.assign({}, row)
      console.log(this.updataDataForm)
      // 其他编辑逻辑...
    },
    cancelUpdate() {
      this.editDialogVisible = false
    },
    confirmUpdate() {
      const updataForm = JSON.stringify(this.updataDataForm)
      axios.post('http://localhost:8080/stus/${this.updataDataForm.id}', updataForm, {
        headers: {

          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          // 处理响应数据
          this.loadData()
          console.log(response)
        })
        .catch(error => {
          // 处理错误
          console.error(error)
        })

      this.editDialogVisible = false
    },
    handleEditDialogClose() {
      // 处理编辑对话框关闭
      this.editDialogVisible = false
    },
    // 表头查询
    onSubmit() {
      const selectForm = JSON.stringify(this.formInline)
      console.log(selectForm)
      axios.post('http://localhost:8080/stus/search', selectForm, {
        headers: {

          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          // 处理响应数据
          this.tableData = response.data.data.rows
        })
        .catch(error => {
          // 处理错误
          console.error(error)
        })
    },
    // 删除
    showDeleteDialog(item) {
      this.selectedItem = item
      this.dialogVisible = true
    },
    cancelDelete() {
      this.dialogVisible = false
    },
    confirmDelete() {
      const id = this.selectedItem.id // 假设选中的项有一个id属性
      axios.delete(`http://localhost:8080/stus/${id}`)
        .then(response => {
          // 删除成功后的处理逻辑，例如刷新数据列表
          console.log(response)
          this.dialogVisible = false // 关闭删除对话框
          this.fetchData() // 重新获取数据，刷新列表
        })
        .catch(error => {
          // 处理删除失败的情况，例如显示错误信息
          console.error(error)
        })
    },
    handleDialogClose() {
      // 关闭删除对话框时的处理逻辑
      // 可在这里重置一些状态
    },
    fetchData() {
      const requestData = {
        pageNum: 1,
        pageSize: 5
      }

      axios.post('http://localhost:8080/stus/search', requestData, {
        headers: {

          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          // 处理响应数据
          this.tableData = response.data.data.rows
        })
        .catch(error => {
          console.log(error)
        })
    },
    // 增加
    showAddDialog() {
      this.addDialogVisible = true
    },

    cancelAdd() {
      this.addDialogVisible = false
      this.resetNewDataForm()
    },
    confirmAdd() {
      // 执行添加数据的操作，例如发送请求到后端
      const jsonData = JSON.stringify(this.newDataForm)
      console.log(jsonData)
      axios.post('http://localhost:8080/stus', jsonData, {

        headers: {

          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          // 处理响应数据
          console.log(response.data)
        })
        .catch(error => {
          // 处理错误
          console.error(error)
        })
      // 可根据实际情况进行修改

      this.addDialogVisible = false
      // this.resetNewDataForm();
    },
    handleAddDialogClose() {

    },
    resetNewDataForm() {
      this.newDataForm = {
        name: '',
        age: ''
      }
    },

    // 分页
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
      this.pageNum = val
      this.loadData()
    },

    loadData() {
      // 根据当前页码和每页显示的条目数加载数据
      const requestData = {
        pageNum: this.pageNum,
        pageSize: 5
      }
      axios.get('http://localhost:8080/stuscount')
        .then(response => {
          // 处理响应数据
          this.count = response.data
        })
        .catch(error => {
          console.log(error)
        })
      axios.post('http://localhost:8080/stus/search', requestData, {
        headers: {

          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          // 处理响应数据
          this.tableData = response.data.data.rows
        })
        .catch(error => {
          console.log(error)
        })
    }

  }

}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
.search-form {
  display: flex;
  flex-wrap: wrap;
  right: 50px;
  align-items: flex-start;

}
.page-footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: #fffdfd;
}

.pagination-wrapper {
  padding: 10px;
  text-align: center;
}
.el-table {
  left: 50px;
  width: 100%;
}

</style>
