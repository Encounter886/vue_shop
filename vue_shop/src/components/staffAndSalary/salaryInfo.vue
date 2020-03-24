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

          <el-col :span="8">
          <el-input placeholder="请输入内容" v-model="query" clearable>
            <el-button slot="append" icon="el-icon-search" @click="findBy">查询员工信息</el-button>
          </el-input>
        </el-col>

        <el-col :span="4">
          <el-button type="primary" @click="able=!able">工资发放</el-button>
        </el-col>
      </el-row>
      <!-- 用户列表区域 -->
      <el-table :data="foodlist" border stripe height="400">
        <el-table-column type="index" label="序号"></el-table-column>
        <!-- <el-table-column label="员工id" prop="staffId"></el-table-column> -->
        <el-table-column label="姓名" prop="staffName"></el-table-column>
        <!-- <el-table-column label="性别" prop="staffSex"></el-table-column>
        <el-table-column label="年龄" prop="staffAge"></el-table-column> -->
        <el-table-column label="电话" prop="staffPhone"></el-table-column>
        <el-table-column label="银行账户" prop="staffAccountNum"></el-table-column>
        <el-table-column label="账户余额" prop="staffAccountMoney"></el-table-column>

        <el-table-column label="发放金额" width="220px" >
          <template slot-scope="scope">  
           <el-input placeholder="请输入金额" type="text" v-model="scope.row.pmoney"  clearable :disabled="able">
            <el-button  type="primary" slot="append"  
            @click="pushMoney(scope.row.staffId,scope.row.pmoney)" :disabled="able">发放</el-button>
          </el-input>
        
          </template>
        </el-table-column>
      </el-table>
    </el-card>

 


  </div>
</template>

<script>
export default {
  data() {
    return {
       able:true,
      foodlist: [       
      ],
       options: [{
          value: '1',
          label: '按照员工姓名模糊查询'
        },  {
          value: '2',
          label: '查询所有'
        }, {
          value: '3',
          label: '按照员工id查询'
        }],
          query: '',
        value: '',

      }
    }
  ,
  created() {},
  mounted(){
  },
  methods: {
      findBy(){
             console.log("value:"+this.value+" query:"+this.query);
             if(this.value==1){
                  this.findByName(this.query);
             } else if(this.value==2){
                    this.findAddFood();
             }
             else if(this.value==3){
                this.findById(this.query);
             }else{
                this.$message.info('未知错误')
             }
      },
      findByName(name){
          if(name===''){
            this.$Message.error("请补全信息");
            console.log(this.name+" --");
        return;
                 }
         this.axios.post(
          "findStaffByName", this.qs.stringify({
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
       findById(id){
           if(id===''){
            this.$Message.error("请补全信息");
            console.log(this.id+" --");
        return;
                 }
         this.axios.post(
          "findStaff", this.qs.stringify({
            id:id
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
      findAddFood(){
     this.axios.post(
          "findStaff", this.qs.stringify({
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

   pushMoney(id,money){
     console.log("开始转账+id= "+id+" money"+money);
      this.axios.post(
          "pushMoney", this.qs.stringify({
            id:id,
            money:money
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                this.$Message.success("转账成功");
                this.findAddFood();
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
   }
     
     
  }
}
</script>

<style lang="less" scoped>


</style>
