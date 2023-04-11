<template>
  <div style="height: 100vh">
    <el-container>
      <el-header>
        <Header></Header>
      </el-header>
      <el-header>
        <!-- 导航栏 -->
        <!--
        <div class="main-navigation">
          <div class="main-nav">
            <div class="row">
              <div class="collapse navbar-collapse" id="main-menu">
                <ul class="menu nav navbar-center">
                  <li><a href="index">首页</a></li>
                </ul> </div> </div> </div></div>
                -->
      </el-header>
      <el-container>
        <el-side style="min-width:60%;">
          <div class="content">
            <div class="TopPic"></div>
            <div class="contentWrap">
              <div class="colum_1">
                <h1>自考实践考核报名后台管理系统</h1>
                <p class="introTxt">本系统用于自考大学专业的社会人士或在读学生等群体的报名、管理和结果查询等。
                  <br /><br />
                  <span style="color:#da4837;font-weight:bold">本系统严禁任何人以非法手段入侵，否则将采取法律手段进行处理。 </span>
                      《中华人民共和国刑法》<br />
                  第二百八十六条【破坏计算机信息系统罪】违反国家规定，对计算机信息系统功能进行删除、修改、增加、干扰，造成计算机信息系统不能正常运行，
                  <br /> <span style="color:#da4837;font-weight:bold">后果严重的，处五年以下有期徒刑或者拘役；后果特别严重的，处五年以上有期徒刑。 </span>
                </p>
                <br><br><br><br><br><br>
              </div> </div>

          </div>
        </el-side>
        <el-main>
          <!--
          prop：表单域 model 字段，在使用 validate、resetFields 方法的情况下，该属性是必填的
          -->
          <!-- direction="vertical"-->
          <div class="table">
            <div class="sontable">
              <h2 style="margin-top: 10px">管理员登录</h2>
              <span class="STYLE2" style="font-size:14px;color:#CC0000;" >&nbsp;{{error_msg}}</span>
              <el-form :model="param" :rules="rules" ref="adminlogin" label-width="80px">
                <el-form-item label="用户名" prop="username">
                  <el-input type="text" placeholder="请输入账号" size="small"
                            prefix-icon="el-icon-user"
                            v-model="param.username"></el-input>
                </el-form-item>
                <el-form-item label="密码" prop="password">
                  <el-input type="password" placeholder="请输入密码" size="small" show-password prefix-icon="el-icon-lock"
                            v-model="param.password"></el-input>
                </el-form-item>

                <el-form-item label="验证码" prop="yzm">
                  <el-col :span="12" >
                    <el-input type="text" placeholder="验证码" prefix-icon="el-icon-picture" style="width: 100px"
                    v-model="param.yzm"></el-input>
                  </el-col>
                  <el-col :span="12">
                    <Canvas></Canvas>
                  </el-col>
                </el-form-item>

                <div style="margin-left: 26px">
                  <el-col :span="12" >
                    <el-button type="primary" @click="Submit">登录</el-button>
                  </el-col>
                  <el-col :span="12">
                    <el-button type="primary" @click="goHome">返回首页</el-button>
                    <!--
                    <el-space wrap size="9px">
                    </el-space>
                    -->
                  </el-col>
                </div>
              </el-form>
            </div>
          </div>
        </el-main>
      </el-container>
      <el-foot>
        <div style="text-align: center">
          <el-link type="default" style="text-align: center" @click="goHome">考生登录</el-link>
        </div>
        <Footer></Footer>
      </el-foot>
    </el-container>
  </div>
</template>

<script>
import Canvas from "@/components/Canvas";
import Header from "@/components/Header";
import Footer from "@/components/Footer";

export default {
  //管理员登陆
  name: "AdminLogin",
  components: {Canvas, Header, Footer},
  data(){
    return {
      param:{
        username:"",
        password:"",
        yzm:"",
        error_msg:"",
      },
      //定义校验规则
      //定义校验规则
      rules:{
        username:[
          { required:true,message:"请输入用户名",trigger:["blur"]},
          { pattern: /^[\u4e00-\u9fa5a-zA-Z0-9]+$/,message: '用户名不能出现特殊字符',trigger:["blur","change"] },
        ],
        password:[
          {required:true,message:"请输入密码",trigger:["blur"]},
          { min: 6, message: '密码不能小于6位', trigger: ["blur","change"] },
          { pattern: new RegExp(/^\S*$/, 'g'), message: '密码不能出现空格',trigger:["blur","change"] }
        ],
        yzm:[
          {required:true,message:"请输入验证码",trigger:["blur"]},
          { pattern: /^[\u4e00-\u9fa5a-zA-Z0-9]+$/,message: '验证码不能出现特殊字符',trigger:["blur","change"] },
          { min: 4, max: 4, message: '请输入4位验证码', trigger: ["blur","change"] }
        ],
      }
    }

  },
  methods: {
    Submit() {
      // 先判断表单是否填写正常
      let error = 0;
      this.$refs.adminlogin.validate((valid) => {
        if (!valid) {
          //console.log("As")
          this.$message({
            type: 'error',
            message: '请先正确填写登录信息'
          });
          error = 1;
          return;
        } else {
          //验证通过调用保存接口
        }
      });
      if( error == 1 )  return;

      //开启loading
      const load = this.$loading({
        lock: true,
        text: '管理员登录中……',
        spinner: 'el-icon-loading',
        background: 'rgba(0, 0, 0, 0.7)'
      });

      //解决axios提交到后端为null的情况
      let params = new URLSearchParams();
      params.append('username', this.param.username);
      params.append('password', this.param.password);
      params.append('yzm', this.param.yzm);

      this.axios({
        method: 'post',
        url:"adminlogin",
        withCredentials:true,
        data:params,
        dataType:'JSONP',  // 处理Ajax跨域问题,
      }).then((response) => {
        load.close()
        if( response.data['msg'] == "ok"){        //账户密码正确
          //存储token  存到浏览器的cookie中，key=oatoken，过期时间1天
          this.$cookies.set("token",response.data['token'],'1d')
          // 设置全局user
          this.$store.commit('adminlogin', response.data)
          this.$router.push("/adminsuccess/adminindex");        //跳转到成功页面
        }else{          //登录失败
          this.$forceUpdate()     //更新到页面
          this.error_msg = response.data['error']
        }
      })
    },
    ///返回首页
    goHome() {
      this.$router.push("/index");
    },
  }
}
</script>

<style lang="less" scoped>
//按钮关系
.relabtn{
  margin-left: 20px;
}
//表单那块——登陆
.table{
  background-color: #eaedf1;
  margin-top:20px;
  height: 370px;
  font-size:14px;
  color:#424242;
  line-height:34px;
  //解决自适应,使登录框永远在右边那块
  max-width: 400px;
  z-index: 2;       //z-index 属性指定一个元素的堆叠顺序。
  width: 100%;
  position: absolute;;
  right: 40px;
}
.sontable{
  margin-top:20px;
  margin-left: 20px;
}
.regSection
{
  display:inline-block;
  padding:0 15px;
  line-height:30px;
  font-weight:bold;
  font-size: 14px;
}
.headerWrap {
  height: 65px;
  margin: auto;
  padding-top: 30px;
  width: 980px;
}
.content {
  margin: auto;
  width: 980px;
}
.contentWrap
{
  padding-top:25px;
}

.colum_1 {
  margin-left: 10vh;
  float: left;
  _display: inline;
  width: 634px;
}

.colum_2 {

  float: left;
  _display: inline;
  width: 326px;
  margin-left: 20px;

}
#h1{
  font-size: 20px;
}
.introTxt {
  border-bottom: 1px solid #DDDDDD;
  color: #666;
  padding: 20px 0;
  margin:0 20px;
  line-height:20px;
  font-size: 17px;
}
.blue-bg{
  width: 100%;
  height: 130px;
  background: #017FCA;
  position: absolute;
  top: 0;
  left: 11px;
  z-index: 0}/*layout-bg*/

.main-navigation ul li a:hover{background: #4AB8FF !important;}
.main-navigation ul li a:active{background: #4AB8FF !important;}

/* main-navigation*/
.main-navigation{width: 100%;height: 50px;text-align: center;background: #1B4568;}
.main-navigation .main-nav{width: 1170px;margin: 0px auto;}
.main-navigation .menu li{height: 50px;font-size: 16px;text-align: center;width: 150px;list-style: none;display: inline-block;position: relative;}
.main-navigation .menu li.nav-current{background: #4AB8FF;}
.main-navigation .menu li:hover{background: #4AB8FF;}
.main-navigation .menu li:active{background: #4AB8FF;}
.main-navigation .menu li a{color: #ffffff;line-height: 3em;display: block;padding: 0px;}
.main-navigation .menu li a:visited{background: #4AB8FF;}
.main-navigation .menu li:hover>a{text-decoration: none;}
.main-navigation .menu li ul{visibility: hidden;background: #4AB8FF;padding: 7px 0px;margin: 0;position: absolute;text-align: left;padding-left: 20px;padding-right: 0px;top: 120%;width: 160px;z-index: 999;border-top: none;}
.main-navigation .menu li ul li{width: 200px;text-align: left;display: block;margin: 0;}
.main-navigation .menu li ul li a{/* line-height: 2.5em;*/color: #505050;}
.main-navigation .menu li ul:hover>a{color: #e67e22;}
.main-navigation .menu li:hover ul{visibility: visible;opacity: 1;/* filter: alpha(opacity=100);*/top: 100%;}
.main-navigation .row { margin: 0; }

.el-form{
  width: 250px;

}
.el-input{
  width:240px;
}

.el-container{
  /*设置内部填充为0，几个布局元素之间没有间距*/
  padding: 0px;
  /*外部间距也是如此设置*/
  margin: 0px;
  height:100%;
  width:100%;

}
/*
.el-header {
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  background-color: #017FCA;
  align-items: center;
  color: #fff;
  //font-size: 20px;
  //标题的字体及大小
  font-size:40px;
  font-family: 华文行楷;
  height: 130px;
  > div {
    display: flex;
    align-items: center;
    img {
      width: 110px;
      height: 110px;
      border-radius: 50%;
    }
    span {
      margin-left: 15px;
    }
  }
}*/
.el-aside {
  background-color: #333744;
.el-menu {
  border-right: none;
}
}
.el-main {

}
</style>