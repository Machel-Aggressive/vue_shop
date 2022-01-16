<template>
  <el-container class="home-container">
    <el-header>
      <div>
        <img src="../assets/yasuo.jpg" style="height: 60px" alt="" />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#ffd04b"
          :unique-opened="true"
          :collapse="isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
          <el-submenu
            :index="'/' + item.path"
            v-for="item in menulist"
            :key="item.id"
          >
            <template slot="title">
              <i :class="iconObj[item.id]"></i>
              <span>{{ item.authName }}</span>
            </template>
            <el-menu-item
              :index="'/' + subItem.path"
              v-for="subItem in item.children"
              :key="subItem.id"
              @click="saveNavState('/' + subItem.path)"
            >
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{ subItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
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
  data() {
    return {
      menulist: [],
      iconObj: {
        125: "el-icon-s-custom",
        103: "el-icon-message-solid",
        101: "el-icon-s-goods",
        102: "el-icon-s-shop",
        145: "el-icon-s-data",
      },
      isCollapse: false,
      activePath: ''
    };
  },
  methods: {
    logout() {
      window.localStorage.removeItem("token");
      this.$router.push("/login");
    },
    async getMenuList() {
      const { data: result } = await this.$http.get("menus");
      console.log(result);
      if (result.meta.status != 200)
        return this.$message.error(result.meta.msg);
      this.menulist = result.data;
    },
    toggleCollapse() {
        this.isCollapse = !this.isCollapse
    },
    saveNavState(activePath) {
        window.sessionStorage.setItem('activePath', activePath)
        this.activePath = activePath
    }
  },
  created() {
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem('activePath')
  },
};
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  color: #eaedf1;
  font-size: 20px;
  align-items: center;
  > div {
    display: flex;
    align-items: center;
    > span {
      margin-left: 15px;
    }
  }
}
.el-aside {
  background-color: #333744;
  .el-menu {
      border-right: none;
  }
  .toggle-button {
      background-color: #4A5064;
      color: #eaedf1;
      font-size: 10px;
      line-height: 24px;
      text-align: center;
      cursor: pointer;
      letter-spacing: 0.2em;
  }
}
.el-main {
  background-color: #eaedf1;
}
</style>