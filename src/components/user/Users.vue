<template>
    <div>
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>用户管理</el-breadcrumb-item>
        <el-breadcrumb-item>用户列表</el-breadcrumb-item>
      </el-breadcrumb>
      <el-card class="box-card">
        <el-row :gutter="20">
          <el-col :span = '8'>
            <el-input placeholder="请输入用户名" class="input-with-select">
              <el-button slot="append" icon="el-icon-search"></el-button>
            </el-input>
          </el-col>
          <el-col :span = '2'>
            <el-button type="primary">添加用户</el-button>
          </el-col>
        </el-row>
        <template>
          <el-table :data="userList" border stripe>
            <el-table-column type="index" label="#"></el-table-column>
            <el-table-column prop="username" label="姓名" min-width="180"></el-table-column>
            <el-table-column prop="email" label="邮箱" min-width="180"></el-table-column>
            <el-table-column prop="mobile" label="电话" min-width="180"></el-table-column>
            <el-table-column prop="role_name" label="角色" min-width="100"></el-table-column>
            <el-table-column label="状态" min-width="100">
              <template slot-scope="scope">
                <el-switch v-model="scope.row.mg_state"></el-switch>
              </template>
            </el-table-column>
            <el-table-column label="操作" min-width="180">
              <template slot-scope="
              // eslint-disable-next-line vue/no-unused-vars
              scope">
                <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
                <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
                <el-tooltip effect="dark" content="分配角色" placement="top" :enterable="false">
                  <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
                </el-tooltip>
              </template>
            </el-table-column>
          </el-table>
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="queryInfo.pagenum"
            :page-sizes="[1, 2, 5, 10]"
            :page-size="queryInfo.pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total">
          </el-pagination>
        </template>
      </el-card>
    </div>
</template>

<script>
export default {
  name: 'Users',
  data() {
    return {
      queryInfo: {
        query: '',
        pagenum: 1,
        pagesize: 5
      },
      userList: [],
      total: 0
    }
  },
  created () {
    this.getUserList()
  },
  methods: {
    async getUserList() {
      const { data: result } = await this.$http.get('users', { params: this.queryInfo })
      if (result.meta.status !== 200) {
        return this.$message.error('获取用户列表失败')
      }
      this.userList = result.data.users
      this.total = result.data.total
    },
    handleSizeChange(newSize) {
      this.queryInfo.pagesize = newSize
      this.getUserList()
    },
    handleCurrentChange(newPage) {
      this.queryInfo.pagenum = newPage
      this.getUserList()
    }
  }
}
</script>

<style lang="less" scoped>
</style>
