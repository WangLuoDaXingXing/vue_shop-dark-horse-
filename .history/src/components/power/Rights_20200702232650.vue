<template>
  <div>
    <!-- 面包屑导航 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>权限列表</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 卡片视图 -->

  </div>
</template>

<script>
export default {
    data() {
        return {
            // 权限列表
            rightsList: []
        }
    },
    created() {
        // 获取所有权限
        this.getRightsList()
    },
    methods: {
        // 获取权限列表
        async getRightsList() {
            const {data: res} = await this.$http.get('rights/list')
            if(res.meta.status !== 200) {
                return this.$message.error('获取权限列表失败')
            }

            this.rightsList = res
        }
    }
};
</script>

<style lang="less" scoped>
</style>