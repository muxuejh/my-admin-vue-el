<template>
  <el-container class="home-container">
    <el-container>
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <!-- <div class="title">电商后台管理系统</div> -->
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409eff"
          :unique-opened="true"
          :collapse="isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <el-submenu
            :index="item.id + ''"
            v-for="item in menulist"
            :key="item.id"
          >
            <!-- 一级菜单模板 -->
            <template slot="title">
              <i :class="iconsObj[item.id]"></i>
              <span>{{ item.authName }}</span>
            </template>
            <!-- 二级菜单 -->
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
      <el-container>
        <el-header>
          <div class="toggle-button" @click="toggleCollapse">
            <i class="iconfont iconmenu"></i>
          </div>
          <div>
            <div>
              <img src="@/assets/images/header.jpg" alt="" />
              <!-- <span>后台管理系统</span> -->
            </div>
            <el-button type="info" @click="logout" size="mini">退出</el-button>
          </div>
        </el-header>
        <el-main>
          <router-view class="loading"></router-view>
        </el-main>
      </el-container>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      // 左侧菜单数据
      menulist: [],
      iconsObj: {
        125: 'iconfont iconusers',
        103: 'iconfont icon028tijikongjian',
        101: 'iconfont icon006shangpin',
        102: 'iconfont icon025danju',
        145: 'iconfont iconbaobiao',
      },
      // 是否折叠
      isCollapse: false,
      // 被激活的连接地址
      activePath: '',
    }
  },
  created() {
    this.getMenuList()
    // console.log(this.$route.path)
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },

    // 获取所有菜单
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status != 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      // console.log(res)
    },

    // 侧边栏折叠与展开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },

    // 保存连接激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    },
  },
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}

.el-header {
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    img {
      width: 28px;
      height: 28px;
      border-radius: 50%;
      margin-right: 5px;
      vertical-align: middle;
    }
    span {
      margin-left: 15px;
    }
  }
}

.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: none;
  }
}

.el-main {
  background-color: #eaedf1;
}

.iconfont {
  margin-right: 10px;
}

.toggle-button {
  margin-left: 20px;
  font-size: 10px;
  line-height: 24px;
  color: #000;
  text-align: right;
  padding-right: 15px;
  letter-spacing: 0.2em;
  cursor: pointer;
}

.title {
  color: #fff;
  width: 200px;
  padding: 0 20px;
  margin: 10px 0;
}
</style>