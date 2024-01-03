<template>
   <el-card class="cardBox">
      <div class="leftBox">
        <div class="leftBoxTop">
          <div class="title">部门列表</div>
          <el-input
            v-model="filterText"
            class="w-50 m-2"
            size="large"
            placeholder="输入关键字"
            clearable
          />
        </div>
        <div class="leftBoxButtom">
          <el-tree
            ref="treeRef"
            :data="tableData"
            :props="defaultProps"
            default-expand-all
            :filter-node-method="filterNode"
            @node-click="nodeClick"
          />
        </div>
      </div>
    </el-card>
</template>

<script lang="ts" setup>
  import { onBeforeMount, ref, watch } from 'vue'
  import { ElTree, tagEmits } from 'element-plus'
  import { deptData } from '@/mock/system'

  const emits = defineEmits(['treeItem'])
  const tableData = ref(deptData)

  onBeforeMount(() => {
    tableData.value = [...deptData]
  })

  const filterText = ref('')
  const treeRef = ref()

  const defaultProps = {
    children: 'children',
    label: 'deptName',
    value: 'id',
  }

  // 监听输入
  watch(filterText, (val) => {
    treeRef.value.filter(val)
  })

  // 搜索
  const filterNode = (value, data) => {
    console.log(data)
    if (!value) return true
    return data.deptName.includes(value)
  }

  // 点击树形tree获取到数据 
  const nodeClick= (data)=>{
    emits('treeItem',data)
  }
</script>

<style lang="scss" scoped>
.cardBox{
  width: 220px;
  height: 100%;
}
  .leftBox {
      width: 220px;
      display: flex;
      flex-direction: column;
      .leftBoxTop {
        width: 100%;
        // height: 150px;
        .title {
          margin: 0 0 15px;
          font-size: 18px;
          font-weight: 700;
        }
      }
      .leftBoxButtom {
        background-color: pink;
        width: 220px;
        flex: 1;
      }
    }
</style>
