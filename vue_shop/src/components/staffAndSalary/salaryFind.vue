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
            <el-button  type="warning" slot="append"  @click="findBy">查询工资信息</el-button>
          </el-input>
        </el-col>
      </el-row>
      <!-- 用户列表区域 -->
      <el-table :data="foodlist" border stripe height="400">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column label="薪资id" prop="salaryId"></el-table-column> 
        <el-table-column label="发放时间" prop="salaryTime"></el-table-column>
        
        <el-table-column label="发放状态" prop="state"></el-table-column>

      </el-table>
    </el-card>

 


  </div>
</template>

<script>
export default {
  data() {
    return {
       able:'false',
      foodlist: [
      ],
       options: [{
          value: '1',
          label: '按照月份模糊查询'
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
  methods: {
      findBy(){
             console.log("value:"+this.value+" query:"+this.query);
             if(this.value==1){
                  this.findByMonth(this.query);
             } else if(this.value==2){
                    this.findAddFood();
             }
             else if(this.value==3){
                this.findById(this.query);
             }else{
                this.$message.info('未知错误')
             }
      },
      findByMonth(time){
          if(time===''){
            this.$Message.error("请补全信息");
            console.log(this.time+" --");
        return;
                 }
         this.axios.post(
          "findSalaryByMonth", this.qs.stringify({
            month:name
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
          "findSalaryById", this.qs.stringify({
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
          "findAllSalarys", this.qs.stringify({
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
