<template>
  <div>
    <div style="height: 60px; background-color: #fff; display: flex; align-items: center; border-bottom: 1px solid #ddd">
      <div style="flex: 1">
        <div style="padding-left: 20px; display: flex; align-items: center">
          <img src="@/assets/imgs/logo.png" alt="" style="width: 40px">
          <div style="font-weight: bold; font-size: 24px; margin-left: 5px; color: #39bf23">MuseLink-圭臬</div>
        </div>
      </div>
      <div style="width: fit-content; padding-right: 10px; display: flex; align-items: center;">
        <img style="width: 40px; height: 40px; border-radius: 50%" :src="data.user.avatar || 'https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png'" alt="">
        <span style="margin-left: 5px">{{ data.user.name }}</span>
      </div>
    </div>

    <div style="display: flex">
      <div style="width: 200px; border-right: 1px solid #ddd; min-height: calc(100vh - 60px)">
        <el-menu
            router
            style="border: none"
            :default-active="router.currentRoute.value.path"
            :default-openeds="['1', '2']"
        >
          <el-menu-item index="/home">
            <el-icon><HomeFilled /></el-icon>
            <span>系统首页</span>
          </el-menu-item>
          <el-menu-item index="/recomment" v-if="data.user.role === '审核员'">
              <el-icon><Menu /></el-icon>
              <span>审核评论</span>
            </el-menu-item>
          <el-sub-menu index="1" v-if="data.user.role === '管理员'">
            <template #title>
              <el-icon><Menu /></el-icon>
              <span>人员管理</span>
            </template>
            <el-menu-item index="/admin">
              <el-icon><SoldOut /></el-icon>
              <span>管理员管理</span>
            </el-menu-item>
            <el-menu-item index="/reviewer">
              <el-icon><SoldOut /></el-icon>
              <span>审核员管理</span>
            </el-menu-item>
            <el-menu-item index="/user">
              <el-icon><SoldOut /></el-icon>
              <span>用户管理</span>
            </el-menu-item>
        
          </el-sub-menu>
      
          <el-sub-menu index="3" v-if="data.user.role === '管理员'">
            <template #title>
              <el-icon><Memo /></el-icon>
              <span>日志记录</span>
            </template>
            <el-menu-item index="/relog">
              <el-icon><User /></el-icon>
              <span>审核员日志记录</span>
            </el-menu-item>
            <el-menu-item index="/log">
              <el-icon><User /></el-icon>
              <span>系统日志</span>
            </el-menu-item>      
          </el-sub-menu>

          <el-menu-item index="/artifact">
            <el-icon><HomeFilled /></el-icon>
            <span>文物管理</span>
          </el-menu-item>

            <el-menu-item index="/backup" v-if="data.user.role === '管理员'">
              <el-icon><User /></el-icon>
              <span>数据库备份和恢复</span>
            </el-menu-item>
          
          <el-menu-item index="/person">
            <el-icon><User /></el-icon>
            <span>个人资料</span>
          </el-menu-item>
          <el-menu-item index="/password">
            <el-icon><Lock /></el-icon>
            <span>修改密码</span>
          </el-menu-item>
          <el-menu-item index="login" @click="logout">
            <el-icon><SwitchButton /></el-icon>
            <span>退出系统</span>
          </el-menu-item>
        </el-menu>
      </div>

      <div style="flex: 1; width: 0; background-color: #f8f8ff; padding: 10px">
        <router-view @updateUser="updateUser" />
      </div>
    </div>

  </div>
</template>

<script setup>
import { reactive } from "vue";
import router from "@/router";
import {ElMessage} from "element-plus";

const data = reactive({
  user: JSON.parse(localStorage.getItem('system-user') || '{}')
})

if (!data.user?.userId) {
  ElMessage.error('请登录！')
  router.push('/login')
}

const updateUser = () => {
  data.user = JSON.parse(localStorage.getItem('system-user') || '{}')
}

const logout = () => {
  ElMessage.success('退出成功')
  localStorage.removeItem('system-user')
  router.push('/login')
}
</script>

<style scoped>
.el-menu-item.is-active {
  background-color: #e0edfd !important;
}
.el-menu-item:hover {
  color: #1967e3;
}
:deep(th)  {
  color: #333;
}
</style>