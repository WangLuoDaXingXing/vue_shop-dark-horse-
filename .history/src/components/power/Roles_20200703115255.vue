<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>权限管理</el-breadcrumb-item>
      <el-breadcrumb-item>角色列表</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 卡片视图 -->
    <el-card>
      <!-- 添加角色按钮区域 -->
      <el-row>
        <el-col>
          <el-button type="primary">添加角色</el-button>
        </el-col>
      </el-row>

      <!-- 角色列表 -->
      <el-table :data="roleslist" border stripe>
          <!-- 展开列 -->
          <el-table-column type="expand">
              <template slot-scope="scope">
                  <el-row :class="['bdbottom', i1 === 0 ? 'bdtop': '', 'vcenter']" v-for="(item1, i1) in scope.row.children" :key="item1.id">
                      <!-- 渲染一级权限 -->
                      <el-col :span="5">
                          <el-tag>{{ item1.authName }}</el-tag>
                          <i class="el-icon-caret-right"></i>
                      </el-col>
                      <!-- 渲染二级和三级权限 -->
                      <el-col :span="19">
                          <!-- 通过for循环，嵌套渲染二级权限 -->
                          <el-row :class="[i2 === 0 ? '': 'bdtop', 'vcenter']" v-for="(item2, i2) in item1.children" :key="item2.id">
                              <el-col :span="6">
                                  <el-tag type="success">{{ item2.authName }}</el-tag>
                                  <i class="el-icon-caret-right"></i>
                              </el-col> 
                              <el-col :span="18">
                                  <el-tag v-for="item3 in item2.children" :key="item3.id" closable @close="removeRightById(scope.row, item3.id)">{{item3.authName}}</el-tag>
                              </el-col>
                          </el-row>
                      </el-col>
                  </el-row>
              </template>
          </el-table-column>
          <!-- 索引列 -->
          <el-table-column type="index"></el-table-column>
          <el-table-column label="角色名称" prop="roleName">
          </el-table-column>
          <el-table-column label="角色描述" prop="roleDesc">
          </el-table-column>
          <el-table-column label="操作" width="300px">
              <template>
                  <el-button size="mini" type="primary" icon="el-icon-edit">编辑</el-button>
                  <el-button size="mini" type="danger" icon="el-icon-delete">删除</el-button>
                  <el-button size="mini" type="warning" icon="el-icon-setting">分配角色</el-button>
              </template>
          </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      roleslist: []
    };
  },
  created() {
    this.getRolesList();
  },
  methods: {
    async getRolesList() {
      const { data: res } = await this.$http.get("roles");
      if(res.meta.status !== 200) {
        return this.$message.error('获取角色列表')
      }

      //   this.$message.success('获取角色列表成功')
      this.roleslist = res.data
    },
    // 根据id删除对应的权限
    async removeRightById(role, rightId) {
        // 弹框提示用户是否删除
        const confirmResult = await this.$confirm('此操作会永久删除改权限，是否继续？', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        }).catch(err => err)

        if(confirmResult !== 'confirm') {
            return this.$message.info('取消了删除操作')
        }
        
        const {data: res} = await this.$http.delete(`roles/${role.id}/rights/${rightId}`)

        if(res.meta.status !== 200) {
            return this.$message.error('删除权限失败！')
        }

        this.getRolesList()
    }
  }
};
</script>

<style lang="less" scoped>
.el-tag {
    margin: 7px;
}

.bdtop {
    border-top: 1px solid #eee;
}

.bdbottom {
    border-bottom: 1px solid #eee;
}

// 纵向上居中对齐
.vcenter {
    display: flex;
    align-items: center;
}
</style>