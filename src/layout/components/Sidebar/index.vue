<template>
  <div class="sidebar-container" :class="{ 'has-logo': themeConfig.showLogo }">
    <Logo :isCollapse="isCollapse" v-if="themeConfig.showLogo" />
    <el-scrollbar wrap-class="scrollbar-wrapper">
      <el-menu
        :default-active="activeMenu"
        background-color="#304156"
        text-color="#bfcbd9"
        :unique-opened="SettingStore.themeConfig.uniqueOpened"
        :collapse-transition="false"
        class="el-menu-vertical-demo"
        :collapse="isCollapse"
        :default-openeds="allSubMenuPaths"
      >
        <!-- el-menu 打开所有子菜单  -->
        <!-- :default-openeds="allSubMenuPaths" -->
        <SubItem v-for="route in permission_routes" :key="route.path" :item="route" />
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script lang="ts" setup>
  import Logo from './components/Logo.vue'
  import SubItem from '../SubMenu/SubItem.vue'
  import { useSettingStore } from '@/store/modules/setting'
  import { usePermissionStore } from '@/store/modules/permission'
  import { computed, ref } from 'vue'
  import { useRoute } from 'vue-router'
  import { pa } from 'element-plus/es/locale'

  // 在setup中获取store
  const route = useRoute()
  const PermissionStore = usePermissionStore()
  const SettingStore = useSettingStore()

  // 是否折叠
  const isCollapse = computed(() => !SettingStore.isCollapse)
  // 设置
  const themeConfig = computed(() => SettingStore.themeConfig)

  // 获取到所有的路由信息
  const permission_routes = computed(() => PermissionStore.permission_routes)

  // 获取所有子菜单的路径,打开所有子菜单
  const allSubMenuPaths = computed(() => {
    return permission_routes.value.map((route) => route.path)
  })

  const activeMenu = computed(() => {
    const { meta, path } = route

    if (meta.activeMenu) {
      return meta.activeMenu
    }
    return path
  })
</script>

<style lang="scss">
  .el-menu-vertical-demo:not(.el-menu--collapse) {
    height: 100%;
  }
</style>
