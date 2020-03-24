<template>
  <div>
    <!-- 卡片视图区域 -->
    <el-card>
        
 
     <div class="images-border">
 <img src="./zhifubao.jpg"  alt="">
 <img src="./wechat.png"  alt="">

 <el-row :gutter="28">
   <el-col :span="5"> <el-input placeholder="请输付款人" v-model="name" clearable>  </el-input> </el-col>
          <el-col :span="6">
            
          <el-input placeholder="请输入金额" type="number" v-model="money" clearable required>
            <el-button slot="append"  type="primary" @click="payMoney" >付款</el-button>
          </el-input>
        </el-col>
      </el-row>
     </div>
    </el-card>
   
  </div>
</template>

<script>
export default {
  data() {
    return {
      name:'',
      money:'',
    } 
  },
  methods:{
    //PaymentController里的接口
    payMoney(){
      if(this.name===''||this.monney===null){
        this.$Message.error("请补全信息");
        console.log(this.name+" --"+this.money);
        return;
      };
      this.axios.post(
          "payMoney", this.qs.stringify({
            name:this.name,
            money:this.money
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                  this.$Message.success("付款成功");
           }else{
            console.log(res.data);
                this.$Message.error("付款失败");
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
    },

}
</script>

<style lang="less" scoped>
.images-border{
    position:relative;
    width:100%;
    height:450px;
}
img{
    margin:20px;
    width:40%;
    height:80%;
}
.el-input-group {
  
    margin-left: 400px;
}
.el-input {
    margin-left: 200px;
}
</style>
