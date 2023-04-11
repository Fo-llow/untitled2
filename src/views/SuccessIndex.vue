<template>

  <div style="margin-left: 60px" >
    <el-carousel indicator-position="outside" style="position: relative; width: 1000px;" height="550px" >
      <el-carousel-item>
        <div style="color: #000000;text-align: center;overflow: hidden;width: 100%;">
          <img  style="width: 100%; height: auto;" src="../assets/cs1.jpg"/>
        </div>
      </el-carousel-item>

      <el-carousel-item>
        <div style="color: #000000;text-align: center;overflow: hidden;width: 100%;">
          <img  style="width: 100%; height: auto;" src="../assets/cs3.jpg"/>
        </div>
      </el-carousel-item>
      <el-carousel-item>
        <div style="color: #000000;text-align: center;overflow: hidden;width: 100%;">
          <img  style="width: 100%; height: auto;" src="../assets/cs4.png"/>
        </div>
      </el-carousel-item>
      <el-carousel-item>
        <div style="color: #000000;text-align: center;overflow: hidden;width: 100%;">
          <img  style="width: 100%; height: auto;" src="../assets/cs5.png" />
        </div>
      </el-carousel-item>

    </el-carousel>

    <el-table
        :data="AnnounceData"
        style="width:1050px"
        :header-cell-style="{background:'#3b6496',color:'#fff'}">
      <el-table-column
          prop="link"
          label="通知公告"
          width="950">
        <template #default="scope">
            <el-link @click="LookAnnounce(AnnounceData[scope.$index].link)" target="_blank" :title=AnnounceData[scope.$index].title>{{"【"+AnnounceData[scope.$index].time+"】"+AnnounceData[scope.$index].title}}</el-link>
        </template>
        <!-- <span>名字</span> -->
      </el-table-column>

      <el-table-column
          prop=""
          width="80">
        <template slot="header"  >
          <!-- <span>名字</span> -->
          <!--          <a href="" target="_blank" class="buttonText">更多</a>-->
          <el-link type="primary" style="text-align: center; color:#fff; font-weight: 700"  @click="goMore">更多</el-link>

        </template>

      </el-table-column>
    </el-table>

  </div>
</template>

<script>
export default {
  //考生成功后的首页
  name: "SuccessIndex",
  data(){
    return {
      AnnounceData:[],
    }
  },
  created() {
    // 进行身份验证（可以解决刷新和直接进入页面问题）
    this.$public.tokenlogin2(this.$cookies,this.axios,this.$store,this.$router,this.$message)
  },
  mounted() {
    this.GetAnnounce();
  },
  methods:{
    goMore(){
      this.$router.push("/more");
    },
    //获得公告
    GetAnnounce(){
      let param = new URLSearchParams();
      param.append("token",this.$cookies.get("token"))
      this.axios.post("showannounce",
          param
      ).then((response) => {
        //console.log(response.data)
        if (response.data['msg'] == "ok") {
          //this.tableData = response.data["announce"]
          this.AnnounceData = []
          //最多显示五条
          let length = response.data["announce"].length ;
          if( length > 5 )  length = 5;

          for( let i = 0 ; i < length ;  i++ ) {
            this.AnnounceData.push({
              title: response.data["announce"][i]['title'],
              time: response.data["announce"][i]['time'],
              link:"/announcement?title="+response.data["announce"][i]['title']+"&time="+ response.data["announce"][i]['time']+"&token="+this.$cookies.get("token"),
            })
          }
        }
      })
    },
    LookAnnounce(link){
      this.$router.push(link);
    },
  }
}
</script>

<style scoped>
#main {
  float: left;
  width: 680px;
}
.colblock h2 .more {
  line-height: 20px;
}
.r {
  float: right;
}
.colblock {

  background-color: rgb(224, 223, 220);
  background-image: none;
  border-color: rgb(200, 196, 192);
}
.colblock {
  margin-bottom: 25px;
  overflow: hidden;
  font-family: Microsoft Yahei;
  background: #FFF;
  background-color: rgb(255, 255, 255);
  background-image: none;
  border: solid #dedede;
  border-top-color: rgb(222, 222, 222);
  border-top-width: medium;
  border-right-color: rgb(222, 222, 222);
  border-right-width: medium;
  border-bottom-color: rgb(222, 222, 222);
  border-bottom-width: medium;
  border-left-color: rgb(222, 222, 222);
  border-left-width: medium;
  border-width: 0 1px 1px;
  border-radius: 4px;
}
.cl {
  zoom: 1;
}
</style>