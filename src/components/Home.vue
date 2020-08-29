<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <i class="weixin"><img src="../assets/me.jpg" alt="wx"></i>
        <span><i class="el-icon-back"/> Add me <i class="i">|</i> 后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu background-color="#1e274b" text-color="#fff" active-text-color="rgb(122, 246, 255)" unique-opened :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">
          <!-- 一级菜单 -->
          <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
            <!-- 一级菜单的模板区域 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{item.authName}}</span>
            </template>

            <!-- 二级菜单 -->
            <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)">
              <template slot="title">
                <!-- 图标 -->
                <i :class="iconsObj_s[subItem.id]"></i>
                <!-- 文本 -->
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容主体 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import {routes} from '../router'
export default {
  data() {
    return {
      // 左侧菜单数据
      menulist: [],
      //一级菜单图标
      iconsObj: {
        '125': 'iconfont icon-user',
        '103': 'iconfont icon-tijikongjian',
        '101': 'iconfont icon-shangpin',
        '102': 'iconfont icon-danju',
        '145': 'iconfont icon-baobiao'
      },
      //二级菜单图标
      iconsObj_s:{
        '110': 'el-icon-menu',
        '111': 'iconfont icon-user',
        '112': 'el-icon-error',
        '104': 'el-icon-plus',
        '115': 'el-icon-info',
        '121': 'el-icon-edit',
        '107': 'el-icon-date',
        '146': 'el-icon-upload'
      },
      // 是否折叠
      isCollapse: false,
      // 被激活的链接地址
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      // console.log(res)
    },
    // 点击按钮，切换菜单的折叠与展开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    // 保存链接的激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
 .weixin{
   position:relative;
   cursor: pointer;
 }
.weixin::after{
  content: url(../assets/wx.png);
  position: absolute;
  right: -65px;
  top: -5px;
  z-index: 99;
  width:120px;
  height: 120px;
  // border: 5px solid #0095ba;
  border-radius: 4px;
  transform-origin: top right;
  transform: scale(0);
  opacity: 0;
  -webkit-transition: all .4s ease-in-out;
  -o-transition: all .4s ease-in-out;
  transition: all .4s ease-in-out;
}
.weixin:hover::after{
  transform:scale(1);
  opacity: 1;
}
.i{
  font-style: normal;
  margin: 0 3.5px 0 3px;
}
.home-container {
  height: 100%;
  color: rgb(122, 246, 255);
}
.el-header {
  background-color: #021450;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    >i img{
      width: 55px;
      height: 55px;
      margin-left: 15px;
      border-radius: 50%;
    }
    span {
      margin-left: 15px;
    }
  }
}

.el-aside {
  background-color: #1e274b;
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
  background-color: #323b57;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
