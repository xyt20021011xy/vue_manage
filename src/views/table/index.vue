<template>
  <div class="class-view">
    <el-form
      :inline="true"
      :model="formInline"
      class="demo-form-inline"
      style="margin-top: 20px; margin-left: 50px;"
    >
      <el-form-item label="学号">
        <el-input v-model="formInline.userid" placeholder="学号" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="tableData"
      style="width: 100%"
    >
      <el-table-column
        prop="userid"
        label="学号"
        width="150"
      />
<!--      <el-table-column-->
<!--        prop="name"-->
<!--        label="姓名"-->
<!--        width="150"-->
<!--      />-->
      <el-table-column
        prop="chinese"
        label="语文"
        width="150"
      />
      <el-table-column
        prop="math"
        label="数学"
        width="150"
      />
      <el-table-column
        prop="english"
        label="英语"
        width="150"
      />
      <el-table-column
        prop="other"
        label="其他"
        width="150"
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
              <el-form-item label="语文">
                <el-input v-model="updataDataForm.chinese" />
              </el-form-item>
              <el-form-item label="数学">
                <el-input v-model="updataDataForm.math" />
              </el-form-item>
              <el-form-item label="英语">
                <el-input v-model="updataDataForm.english" />
              </el-form-item>
              <el-form-item label="其他">
                <el-input v-model="updataDataForm.other" />
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="cancelUpdate">取消</el-button>
              <el-button type="primary" @click="confirmUpdate">确定</el-button>
            </div>
          </el-dialog>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Dashboard',
  data() {
    return {
      count: 10,
      // 表格数据
      tableData: [],
      // 表头查询
      formInline: {
        userid: ''
      },
      // newDataForm: {
      //   userid: '',
      //   name: '',
      //   gender: '',
      //   cclass: '',
      //   createTime: '',
      //   qq: '',
      //   phone: ''
      //
      // },
      // 编辑
      updataDataForm: {
        userid: '',
        chinese: '',
        math: '',
        english: '',
        other: ''
      },
      editDialogVisible: false, // 编辑对话框可见性
      editedData: null // 存储当前正在编辑的数据
    }
  },

  mounted() {
    axios.get('http://localhost:8080/sc')
      .then(response => {
        // 处理响应数据
        console.log(response)
        this.tableData = response.data.data
        console.log(this.tableData)
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
      axios.post('http://localhost:8080/sc', updataForm, {
        headers: {

          'Content-Type': 'application/json'
        }
      })
        .then(response => {
          // 处理响应数据
          axios.get('http://localhost:8080/sc')
            .then(response => {
              // 处理响应数据
              console.log(response)
              this.tableData = response.data.data
              console.log(this.tableData)
            })
            .catch(error => {
              console.log(error)
            })
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
      const userid = this.formInline.userid
      console.log(userid)
      axios.get('http://localhost:8080/sc/' + userid)
        .then(response => {
          // 处理响应数据
          console.log(response)
          this.tableData = response.data.data
          console.log(this.tableData)
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
