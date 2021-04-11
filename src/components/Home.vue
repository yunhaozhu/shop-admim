<template>
    <el-container class="home-container">
      <el-header>
        <div>
          <span>Dashboard</span>
        </div>
        <el-button size="medium" plain @click="logout">退出</el-button>
      </el-header>
      <el-container>
        <el-aside :width="isCollapse?'64px':'230px'">
          <el-menu
            :default-active="activePath"
            :collapse="isCollapse"
            unique-opened router
            background-color="#ffffff"
            text-color="#33475b"
            active-text-color="#409efd"
            :collapse-transition="false">
            <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
              <template slot="title">
                <i :class="iconsObj[item.id]"></i>
                <span>{{item.authName}}</span>
              </template>
              <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children"
                            @click="saveNavState('/' + subItem.path)"
                            :key="subItem.id">
                <template slot="title">
                  <i class="el-icon-menu"></i>
                  <span>{{subItem.authName}}</span>
                </template>
              </el-menu-item>
            </el-submenu>
            <div class="toggle-button" @click="toggleCollapse">|||</div>
          </el-menu>
        </el-aside>
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      menuList: [],
      activePath: '',
      iconsObj: {
        125: 'el-icon-user-solid',
        103: 'el-icon-s-tools',
        101: 'el-icon-s-goods',
        102: 'el-icon-s-order',
        145: 'el-icon-s-platform'
      },
      isCollapse: false
    }
  },
  created () {
    this.activePath = window.sessionStorage.getItem('activePath')
    this.getMenuList()
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
    },
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
    }
  }
}
</script>

<style lang="less" scoped>
  .home-container{
    height: 100%;
  }
  .el-menu{
    height: 100%;
  }
  .el-header{
    background-color: #2e3f50;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #ffffff;
    font-size: 20px;
    > div {
      display: flex;
      align-items: center;
    }
  }
  .el-aside{
    background-color: #ffffff;
  }
  .el-main{
    background-color: #f5f8fa;
    //#ff7a59 #eaf0f6 #e5f5f8
  }
  .toggle-button {
    background-color: #ffffff;
    line-height: 24px;
    font-size: 10px;
    color: #33475b;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
  }
</style>
