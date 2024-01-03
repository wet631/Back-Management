<template>
  <el-card class="m-dept-side">
    <div class="title">部门列表</div>
    <el-input v-model="filterText" placeholder="输入关键字进行过滤" class="filter-search" />
    <div class="filter-tree">
      <el-scrollbar class="scrollbar">
        <el-tree
          ref="treeRef"
          :data="tableData"
          :props="defaultProps"
          default-expand-all
          :filter-node-method="filterNode"
          @node-click="nodeClick"
        />
      </el-scrollbar>
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
  @import '../index.scss';
</style>
