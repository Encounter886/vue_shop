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
            <el-button slot="append" icon="el-icon-search" @click="findBy">查询出库记录</el-button>
          </el-input>
        </el-col>

        <el-col :span="4">
          <el-button type="primary" @click="addDialogVisible = true">添加出库记录</el-button>
        </el-col>
      </el-row>
      <!-- 用户列表区域 -->
      <el-table :data="foodlist" border stripe height="400">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column label="出库号id" prop="outFoodId"></el-table-column>
        <el-table-column label="出库价值总金额" prop="outFoodPay"></el-table-column>
        <el-table-column label="出库数量" prop="outFoodNum"></el-table-column>
        <el-table-column label="出库时间" prop="outTime"></el-table-column>

        <el-table-column label="操作" width="180px">
          <template slot-scope="scope">
            <!-- 修改按钮 -->
            <el-button type="primary" icon="el-icon-edit" size="mini" @click="showEditDialog(scope.row.outFoodId,scope.row.outFoodPay,scope.row.outFoodNum,scope.row.outTime)"></el-button>
            <!-- 删除按钮 -->
            <el-button type="danger" icon="el-icon-delete" size="mini" @click="removeById(scope.row.outFoodId)"></el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>

 <!-- 添加caipin的对话框 -->
    <el-dialog title="添加用户" :visible.sync="addDialogVisible" width="50%" @close="addDialogClosed">
      <!-- 内容主体区域 -->
      <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="70px">
        <el-form-item label="菜品id" prop="outFoodId">
          <el-input v-model="addForm.outFoodId" ></el-input>
        </el-form-item>
        <el-form-item label="总金额" prop="outFoodPay">
          <el-input v-model="addForm.outFoodPay"></el-input>
        </el-form-item>
        <el-form-item label="数量" prop="outFoodNum">
          <el-input v-model="addForm.outFoodNum"></el-input>
        </el-form-item>
        <el-form-item label="时间" prop="outTime">
          <el-input v-model="addForm.outTime" disabled ></el-input>
        </el-form-item>
      </el-form>
      <!-- 底部区域 -->
      <span slot="footer" class="dialog-footer">
        <el-button @click="addDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addFood">确 定</el-button>
      </span>
    </el-dialog>

<!-- 修改用户的对话框 -->
    <el-dialog title="修改用户" :visible.sync="editDialogVisible" width="50%" @close="editDialogClosed">
      <el-form :model="editForm" :rules="editFormRules" ref="editFormRef" label-width="70px">
            <el-form-item label="出库id">
          <el-input v-model="editForm.outFoodId" disabled></el-input>
        </el-form-item>
        <el-form-item label="总金额">
          <el-input v-model="editForm.outFoodPay" ></el-input>
        </el-form-item>
        <el-form-item label="数量" >
          <el-input v-model="editForm.outFoodNum" ></el-input>
        </el-form-item>
        <el-form-item label="时间" prop="outTime">
          <el-input v-model="editForm.outTime"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="editUserInfo">确 定</el-button>
      </span>
    </el-dialog>
  

  </div>
</template>

<script>
export default {
  data() {
    return {
      foodlist: [           
      ],
       options: [{
          value: '1',
          label: '按照菜品名称模糊查询'
        },  {
          value: '2',
          label: '按照时间模糊查询'
        }, {
          value: '3',
          label: '查询所有'
        }, {
          value: '4',
          label: '按照菜品id查询'
        }],
          query: '',
        value: '',
      // 控制添加对话框的显示与隐藏
     addDialogVisible:false,
     addForm:{
                  outFoodId:'',
               outFoodPay:'',
              outFoodNum:'',
               outTime:new Date()+'',
     },
     addFormRules:{},

       // 控制修改用户对话框的显示与隐藏
      editDialogVisible: false,
      // 查询到的用户信息对象
      editForm: {  outFoodId:'',
               outFoodPay:'',
              outFoodNum:'',
               outTime:'',
       },
      // 修改表单的验证规则对象
      editFormRules: {
      },

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
                    this.findAddFood();
             }
             else if(this.value==4){
                this.findById(this.query);
             }else{
                this.$message.info('未知错误')
             }
      },
      findByName(name){
            this.axios.post(
          "findOutFoodByName", this.qs.stringify({
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
          "findOutFoodByTime", this.qs.stringify({
            time:time
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
                this.axios.post(
          "findOutFoodById", this.qs.stringify({
            id:id
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                    this.foodlist=[];
                    this.foodlist.push(res.data.data);
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
          "findAllOutFood", this.qs.stringify({
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

   // 监听添加用户对话框的关闭事件
    addDialogClosed() {
      this.$refs.addFormRef.resetFields()
    },
    // 点击按钮，添加新用户
    addFood() {
      this.$refs.addFormRef.validate(async valid => {
        if (!valid) return;
         this.axios.post(
          "updateOrAddOutFood",   this.qs.stringify({
               outFoodPay:this.addForm.outFoodPay,
              outFoodNum:this.addForm.outFoodNum,
               outTime:this.addForm.outTime,
                 vegetableId:this.addForm.outFoodId,
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
            this.addDialogVisible=false;
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
    
      })
    },
      showEditDialog(id,pay,num,time){
     console.log("showid:"+id+pay+num+time);

      this.editForm.outFoodId=id;
      this.editForm.outFoodPay=pay;
      this.editForm.outFoodNum=num;
      this.editForm.outTime=time;
      this.editDialogVisible = true
      },
// 监听修改用户对话框的关闭事件
    editDialogClosed() {
      this.$refs.editFormRef.resetFields();
    },
    // 修改用户信息并提交
    editUserInfo() {
      this.$refs.editFormRef.validate(async valid => {
        if (!valid) return;
           this.axios.post(
          "updateOrAddOutFood",  this.qs.stringify({
            outFoodId:this.editForm.outFoodId,
            outFoodPay: this.editForm.outFoodPay,
             outFoodNum: this.editForm.outFoodNum,
              outTime: this.editForm.outTime,
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
      this.editDialogVisible = false;
        // 提示修改成功
        this.$message.success('更新信息成功！');
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
      })
    },
     async   removeById(id){
         console.log("removeById:"+id);
       // 弹框询问用户是否删除数据
      const confirmResult = await this.$confirm(
        '此操作将永久删除该用户, 是否继续?',
        '提示',
        {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }
      ).catch(err => err);

      // 如果用户确认删除，则返回值为字符串 confirm
      // 如果用户取消了删除，则返回值为字符串 cancel
      // console.log(confirmResult)
      if (confirmResult !== 'confirm') {
        return this.$message.info('已取消删除')
      }
this.axios.post(
          "deleteOutFoodById",  this.qs.stringify({
            id:id,
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
        this.$message.success('delelte信息成功！');
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
  
      },
     
  }
}
</script>

<style lang="less" scoped>


</style>
