<template>
    <el-container>
        <!-- 头部区域 -->
        <el-header>Header <el-button type="info" @click="logout">退出</el-button> </el-header>
            <!-- 页面主体区域 -->
            <el-container>
                <!-- 侧边栏 -->
                <el-aside width="200px">Aside</el-aside>
                <!-- 右侧主体 -->
                <el-main>Main</el-main>
            </el-container>
    </el-container>
</template>

<script>
export default {
  methods: {
    logout() {
      window.sessionStorage.clear(); // 清空token
      this.$router.push("/login");
    }
  }
};
</script>

<style lang="less" scoped>
</style>