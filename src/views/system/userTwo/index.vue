<template>
  <div class="container">
    <div class="leftB">
      <userSide @treeItem="treeItem"></userSide>
    </div>
    <div class="rightBox">
      <div class="rightBoxTop">
        <el-card>
          <el-form :inline="true" :model="formInline" ref="ruleFormRef">
            <el-form-item label="用户名" prop="username">
              <el-input v-model="formInline.username" placeholder="请输入用户名" />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="onSubmit" :icon="Search">查询</el-button>
              <el-button @click="reset(ruleFormRef)">重置</el-button>
            </el-form-item>
          </el-form>
        </el-card>
      </div>
      <div class="rightBoxButtom">
        <el-card class="rightBoxButtomCard">
          <div class="addUtil">
            <el-button type="primary" @click="addHandler">
              <el-icon><Plus /></el-icon>
              新增用户
            </el-button>
          </div>
          <div class="tableBox">
            <el-table
              v-loading="loading"
              :data="tableData"
              style="width: 100%; height: 100%"
              border
            >
              <el-table-column prop="username" label="用户名" align="center" width="100" />
              <el-table-column prop="nickname" label="昵称" align="center" />
              <el-table-column prop="sex" label="性别" align="center" />
              <el-table-column prop="role" label="关联角色" align="center" width="120" />
              <el-table-column prop="photo" label="手机号" align="center" width="120" />
              <el-table-column prop="status" label="用户状态" align="center">
                <template #default="scope">
                  <el-switch
                    inline-prompt
                    active-text="启用"
                    inactive-text="禁用"
                    v-model="scope.row.status"
                    @change="changeStatus(scope.row)"
                  />
                </template>
              </el-table-column>
              <el-table-column
                prop="describe"
                :show-overflow-tooltip="true"
                width="180"
                label="用户描述"
                align="center"
              />
              <el-table-column prop="createTime" label="创建时间" align="center" width="180" />
              <el-table-column
                prop="operator"
                label="操作"
                width="200px"
                align="center"
                fixed="right"
              >
                <template #default="scope">
                  <el-button
                    type="primary"
                    size="small"
                    icon="Edit"
                    @click="editHandler(scope.row)"
                  >
                    编辑
                  </el-button>
                  <el-button @click="del(scope.row)" type="danger" size="small" icon="Delete">
                    删除
                  </el-button>
                </template>
              </el-table-column>
            </el-table>
          </div>
          <div class="pagination">
            <el-pagination
              v-model:currentPage="currentPage1"
              :page-size="10"
              background
              layout="total,  prev, pager, next, jumper"
              :total="1000"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
            />
          </div>
        </el-card>
      </div>
    </div>
  </div>
</template>

<script setup>
  import UserSide from './components/userSide.vue'
  import { ref, reactive, onMounted } from 'vue'
  import { ElMessageBox } from 'element-plus'
  import { Search, Plus } from '@element-plus/icons-vue'
  import { userData } from '@/mock/system'

  const tableData = ref(userData)
  const dialogVisible = ref(false)
  const userDialog = ref()
  const ruleFormRef = ref()
  const formInline = reactive({})
  const loading = ref(true)
  const currentPage1 = ref(1)

  const onSubmit = () => {
    loading.value = true
    setTimeout(() => {
      loading.value = false
    }, 1000)
  }

  const reset = (formEl) => {
    loading.value = true
    setTimeout(() => {
      loading.value = false
    }, 1000)
  }

  const addHandler = () => {
    userDialog.value.show()
  }

  const editHandler = (row) => {
    userDialog.value.show(row)
  }

  const del = (row) => {
    ElMessageBox.confirm('你确定要删除当前项吗?', '温馨提示', {
      confirmButtonText: '确定',
      cancelButtonText: '取消',
      type: 'warning',
      draggable: true,
    })
      .then(() => {})
      .catch(() => {})
  }

  const changeStatus = (row) => {
    ElMessageBox.confirm(
      `确定要${!row.status ? '禁用' : '启用'} ${row.username} 账户吗？`,
      '温馨提示',
      {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning',
      },
    )
      .then(async () => {})
      .catch(() => {
        row.status = !row.status
      })
  }

  const handleSizeChange = (val) => {
    console.log(`${val} items per page`)
  }

  const handleCurrentChange = (val) => {
    currentPage1.value = val
    onSubmit()
    console.log('handleCurrentChange')
  }
  const treeItem = (data) => {
    onSubmit()
  }
  onMounted(() => {
    setTimeout(() => {
      loading.value = false
    }, 1000)
  })
</script>

<style lang="scss" scoped>
  .container {
    width: 100%;
    padding: 5px;
    display: flex;
    .rightBox {
      flex: 1;
      display: flex;
      flex-direction: column;
      margin-left: 10px;
      width: calc(100% - 230px);
      .rightBoxTop {
        width: 100%;
        margin-bottom: 5px;
        border-radius: 4px;
        background: white;
        box-shadow: 0 0 12px rgb(0 0 0 / 5%);
      }
      .rightBoxButtom {
        flex: 1;
        display: flex;
        flex-direction: column;
        position: relative;
        .rightBoxButtomCard {
          height: 100%;
        }
        .addUtil {
          margin-bottom: 5px;
          display: flex;
          justify-content: flex-end;
          flex-shrink: 0;
        }
        .tableBox {
          flex: 1;
        }
        .pagination {
          position: absolute;
          bottom: 10px;
          right: 30px;
          width: 100%;
          display: flex;
          justify-content: flex-end;
          padding-top: 20px;
          box-sizing: border-box;
          flex-shrink: 0;
        }
      }
    }
  }
</style>
