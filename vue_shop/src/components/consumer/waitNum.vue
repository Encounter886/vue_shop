<template>
  <div>
    <!-- 卡片视图区域 -->
    <el-card>
         
 <div class="header">
     <el-button type="primary" size="medium" @click="waitNum">排号</el-button>
  <el-button type="success" size="medium" @click="getNum"> 取号</el-button>
 </div>
 <div class="bottom">
  <div class="hao"> <span>当前排/取号为：{{wait}}</span><span class="warning">
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{msg1}}</span></div>
  <div class="hao"> <span>当前叫号为：{{call}}</span><span class="warning">
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{msg2}}</span></div>
 </div>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
       call:'0',
       wait:'0',
       msg1:'',
       msg2:''
     
  }
  },
  methods:{
  //调用DeskController里的方法
  waitNum(){
  this.axios.post(
          "addConsumer", this.qs.stringify({
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
             this.wait=res.data.data;
             this.msg1=res.data.msg;
           }else{
            console.log(res.data);
                this.$Message.error("排队失败");
           }
          }.bind(this)
        )
        .catch(
          function(err) {
            if (err.response) {
              console.log(err.response);
            }
          }.bind(this)
        );
  },
  getNum(){
 this.axios.post(
          "getConsumer", this.qs.stringify({
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
             this.call=res.data.data;
             this.msg2=res.data.msg;
           }else{
            console.log(res.data);
                this.$Message.error("排队失败");
           }
          }.bind(this)
        )
        .catch(
          function(err) {
            if (err.response) {
              console.log(err.response);
            }
          }.bind(this)
        );
  }
  }

}
</script>

<style lang="less" scoped>
.hao{
    margin:20px;
    height:200px;
    font-size:40px;
    text-align:left;
    color:#A3571F;
}
.warning{
    color:red;
    font-size:20px;
}
</style>
