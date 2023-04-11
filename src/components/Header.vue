<template>
  <div class="father">
    <div class="blue-bg" >
      <div class="min-nav">
        <div class="linkContent">
          <el-link href="http://8.134.142.145/index.html"> 二维码登录</el-link>
          <el-divider direction="vertical"></el-divider>
          <el-link href="http://8.134.142.145:8080/dist/">考易通主页</el-link>
          <el-divider direction="vertical"></el-divider>
          <el-link href="http://8.134.142.145:8080/dist/">内部门户</el-link>
        </div>
      </div>
      <li style="margin-top: 15px">
        <img src="../assets/logo.png" class="logo" alt="" style="height: 90px;width: 90px;"/>
        <span class="title">考易通——开放式成人自学考试管理系统</span>
      </li>
      <slot class="slotcss"> </slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  methods:{

  }
}

</script>

<style lang="less" scoped>
@import "../mobile.css";

.father{
  overflow: hidden;     //当内容溢出元素框时所触发的事件
  white-space: nowrap;  //强制同一行内显示所有文本文字，让所有文字内容中一排显示不换行。
  text-overflow: ellipsis;  //当对象内文本一处时显示省略标记(...)。
}
.slotcss{
  /*margin-right: 0px;*/

}
.linkContent{
  text-align: right;
  color:#fff;
}
.linkContent a{color: #fff;}
.linkContent a:hover{color: #a5b9f1;}
.min-nav{
  float:right;
  /*margin-top:10px;*/
  font-size:12px;
  font-family: 宋体;

}
.min-nav ul li a{
  display: block;font-size: 13px;color: #FFF;
  text-decoration-line: none;}
//logo与标题对齐
.logo {
  width: 30%;
  height: 100%;
  display: inline-block;
  box-sizing: border-box;
  vertical-align: middle;
}

.title {
  width: 70%;
  height: 100%;
  margin-top: 25px;
  display: inline-block;
  box-sizing: border-box;
  vertical-align: middle;
}
.item_img{
  width: 40px;
  height: 40px;
}


.blue-bg{
  margin-left: 0px;
  float:left;
  width: 100%;
  height: 130px;
  background: #017FCA;
  position: absolute;
  top: 0;
  left: 0px;
  z-index: 0;
  font-size:36px;
  font-family: 华文行楷;

  color: #fff;
  > div {
    display: flex;
    align-items: center;

    img {
      width: 110px;
      height: 110px;
      border-radius: 50%;
    }

    span {
      /*margin-left: 15px;*/
    }
  }

}
</style>