<template>
  <div>
    <!-- 卡片视图区域 -->
    <el-card>
         
   
      <!-- 搜索与添加区域 -->
      <el-row :gutter="28">
          <el-col :span="4">
           <el-select v-model="value" placeholder="请选择查询方式">
        <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
      </el-select>
          </el-col>

          <el-col :span="10">
          <el-input placeholder="请输入查询内容" v-model="query" clearable>
            <el-button type="primary" slot="append" icon="el-icon-search" @click="findBy">查询消费记录</el-button>
          </el-input>
        </el-col>
      </el-row>
      <!-- 用户列表区域 -->
      <el-table :data="foodlist" border stripe height="400">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column label="付款id" prop="paymentId"></el-table-column>
        <el-table-column label="付款人" prop="paymentPeople"></el-table-column>
        <el-table-column label="付款金额" prop="paymentNum"></el-table-column>
        <el-table-column label="付款时间" prop="paymentTime"></el-table-column>
        </el-table-column>
      </el-table>
    </el-card>

  </div>
</template>

<script>
export default {
  data() {
    return {
      foodlist: [     
      ],
       options: [{
          value: '2',
          label: '按照月份模糊查询'
        },  {
          value: '1',
          label: '按照姓名模糊查询'
        }, {
          value: '3',
          label: '查询所有'
        }
        ],
          query: '',
        value: '',
      // 控制添加对话框的显示与隐藏
      }
    }
  ,
  created() {},
  methods: {
      findBy(){
             console.log("value:"+this.value+" query:"+this.query);
             if(this.value==1){
                  this.findByName(this.query);
             }else if(this.value==2){
                this.findByTime(this.query);
             }
             else if(this.value==3){
                    this.findAll();
             }
            else{
                this.$message.info('未知错误')
             }
      },
      findByName(name){
         this.axios.post(
          "findPaymentByName", this.qs.stringify({
            name:name
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                    this.foodlist= res.data.data;
           }else{
            console.log(res.data);
                 alert("error");
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
      findByTime(time){
this.axios.post(
          "findPaymentByMonth", this.qs.stringify({
            month:time
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                    this.foodlist= res.data.data;
           }else{
            console.log(res.data);
                 alert("error");
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
      findAll(){
this.axios.post(
          "findAllPayment", this.qs.stringify({
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                    this.foodlist= res.data.data;
           }else{
            console.log(res.data);
                 alert("error");
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

   
     
  }
}
</script>

<style lang="less" scoped>


</style>
