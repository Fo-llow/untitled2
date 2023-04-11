<template>
  <div>
    <div style="height: 100vh" >
      <el-container>
        <el-header>
          <Header></Header>
          <div style=" right: 76px;top: 55px; position: absolute;">
            <el-dropdown>
              <div>
                <el-avatar icon="el-icon-user">
                  <i class="el-icon-arrow-down"></i>
                </el-avatar>
                <div style="color: #FFFFFF;text-align: center" > {{ help.name }}</div>
              </div>

              <el-dropdown-menu>
                <el-dropdown-item @click.native="goPer">个人权限</el-dropdown-item>
                <el-dropdown-item @click.native="logout">退出登录</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </div>
        </el-header>
        <el-container>
          <el-side width="200px" style="min-width:200px;max-width:200px;margin-top: 90px;">
            <el-menu
                default-openeds="['1']"
                background-color="#fff"
                text-color="#000000"
                active-text-color="#409EFF"
                :default-active="$route.path"
                router>
              <el-menu-item index="/adminsuccess/adminindex" >
                <i class="el-icon-house"></i>
                首页</el-menu-item>
              <el-submenu index="1">
                <template slot="title">
                  <i class="el-icon-menu"></i>
                  <span slot="title">管理员功能</span>
                </template>
                <el-menu-item index="/adminsuccess/adminmanage">
                  <i class="el-icon-edit"></i>
                  报名管理</el-menu-item>
                <el-menu-item index="/adminsuccess/adminexamine">
                  <i class="el-icon-document-checked"></i>
                  报名审核</el-menu-item>
                <el-menu-item index="/adminsuccess/adminannounce">
                  <i class="el-icon-message"></i>
                  公告发布
                </el-menu-item>
                <el-menu-item @click="warn" >
                  <i class="el-icon-cpu"></i>
                  机房管理</el-menu-item>
                <el-menu-item index="/adminsuccess/adminjurisdict">
                  <i class="el-icon-lock"></i>
                  权限管理
                </el-menu-item>
              </el-submenu>
            </el-menu>
          </el-side>
          <el-main  style="margin-top: 90px">
            <!-- 路由占位符 -->
            <router-view></router-view>
          </el-main>

        </el-container>
        <el-foot>
          <Footer></Footer>
        </el-foot>
      </el-container>
    </div>
  </div>
</template>

<script>

import Header from "@/components/Header";
import Footer from "@/components/Footer";
import ElementUI from "element-ui";

export default {
  name: "AdminSuccess",
  components: {Header, Footer},
  data(){
    return {
      // 数组等类型才能引用传值，后面的变量实际没用
      help: {name: "", other: ""},      // 数组才能引用传参
    }
  },
  created() {
    // 进行身份验证（可以解决刷新和直接进入页面问题）
    this.$public.tokenadminlogin(this.$cookies,this.axios,this.$store,this.$router,this.$message,this.help)
  },
  methods:{
    //提示警告
    warn(){
      //console.log("该功能尚未开发，敬请期待")
      ElementUI.Message({
        message: '该功能尚未开发，敬请期待',
        type: 'error'
      });
    },
    //点击个人权限
    goPer(){
      if( this.$store.state.adminuser.permission != 1 ){
        ElementUI.Message({
          message: '当前权限：【普通管理员】',
          type: 'info'
        });
      }else{
        ElementUI.Message({

          message: '当前权限:【超级管理员】',
          type: 'info'
        });
      }



    },
    //点击退出登陆
    logout(){
      this.$store.commit('adminlogout')     //删除vuex里面的数据
      this.$cookies.remove('token')     //删除cookie里面的token
      //返回首页
      this.$router.push("/adminlogin");
    },

  }
}
</script>

<style scoped>
@import "../public.css";

</style>