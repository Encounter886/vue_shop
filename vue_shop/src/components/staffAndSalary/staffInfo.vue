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
          <el-button type="primary" @click="addDialogVisible = true">添加员工信息</el-button>
        </el-col>
      </el-row>
      <!-- 用户列表区域 -->
      <el-table :data="foodlist" border stripe height="400">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column label="员工id" prop="staffId"></el-table-column>
        <el-table-column label="姓名" prop="staffName"></el-table-column>
        <el-table-column label="性别" prop="staffSex"></el-table-column>
        <el-table-column label="年龄" prop="staffAge"></el-table-column>
        <el-table-column label="电话" prop="staffPhone"></el-table-column>
        <el-table-column label="银行账户" prop="staffAccountNum"></el-table-column>
        <el-table-column label="账户余额" prop="staffAccountMoney"></el-table-column>

        <el-table-column label="操作" width="180px">
          <template slot-scope="scope">
            <!-- 修改按钮 -->
            <el-button type="primary" icon="el-icon-edit" size="mini" 
            @click="showEditDialog(scope.row.staffId,scope.row.staffName,scope.row.staffSex,
            scope.row.staffAge,scope.row.staffPhone,scope.row.staffAccountNum,scope.row.staffAccountMoney)"></el-button>
            <!-- 删除按钮 -->
            <el-button type="danger" icon="el-icon-delete" size="mini" @click="removeById(scope.row.staffId)"></el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>

 <!-- 添加caipin的对话框 -->
    <el-dialog title="添加用户" :visible.sync="addDialogVisible" width="50%" @close="addDialogClosed">
      <!-- 内容主体区域 -->
      <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="70px">
        <el-form-item label="员工id" prop="staffId">
          <el-input v-model="addForm.staffId" disabled></el-input>
        </el-form-item>
        <el-form-item label="姓名" prop="staffName">
          <el-input v-model="addForm.staffName"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="staffSex">
          <el-input v-model="addForm.staffSex"></el-input>
        </el-form-item>
        <el-form-item label="年龄" prop="staffAge">
          <el-input v-model="addForm.staffAge"></el-input>
        </el-form-item>
              <el-form-item label="电话" prop="staffPhone">
          <el-input v-model="addForm.staffPhone"></el-input>
        </el-form-item>
              <el-form-item label="银行账户" prop="staffAccountNum">
          <el-input v-model="addForm.staffAccountNum"></el-input>
        </el-form-item>
              <el-form-item label="账户余额" prop="staffAccountMoney">
          <el-input v-model="addForm.staffAccountMoney"></el-input>
        </el-form-item>
      </el-form>
      <!-- 底部区域 -->
      <span slot="footer" class="dialog-footer">
        <el-button @click="addDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addStaff">确 定</el-button>
      </span>
    </el-dialog>

<!-- 修改用户的对话框 -->
    <el-dialog title="修改用户" :visible.sync="editDialogVisible" width="50%" @close="editDialogClosed">
      <el-form :model="editForm" :rules="editFormRules" ref="editFormRef" label-width="70px">
            <el-form-item label="员工id">
          <el-input v-model="editForm.staffId" disabled></el-input>
        </el-form-item>
   <el-form-item label="姓名" prop="staffName" disabled>
          <el-input v-model="editForm.staffName"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="staffSex" disabled>
          <el-input v-model="editForm.staffSex"></el-input>
        </el-form-item>
        <el-form-item label="年龄" prop="staffAge">
          <el-input v-model="editForm.staffAge"></el-input>
        </el-form-item>
              <el-form-item label="电话" prop="staffPhone">
          <el-input v-model="editForm.staffPhone"></el-input>
        </el-form-item>
              <el-form-item label="银行账户" prop="staffAccountNum">
          <el-input v-model="editForm.staffAccountNum"></el-input>
        </el-form-item>
              <el-form-item label="账户余额" prop="staffAccountMoney">
          <el-input v-model="editForm.staffAccountMoney"></el-input>
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
      // 控制添加对话框的显示与隐藏
     addDialogVisible:false,
     addForm:{
                 staffId:'',
               staffName:'',
              staffSex:'',
               staffAge:'',
               staffPhone:'',
               staffAccountNum:'',//银行账户
               staffAccountMoney:'',//账户余额
     },
     addFormRules:{},

       // 控制修改用户对话框的显示与隐藏
      editDialogVisible: false,
      // 查询到的用户信息对象
      editForm: {   staffId:'',
               staffName:'',
              staffSex:'',
               staffAge:'',
               staffPhone:'',
               staffAccountNum:'',//银行账户
               staffAccountMoney:'',//账户余额
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

   // 监听添加用户对话框的关闭事件
    addDialogClosed() {
      this.$refs.addFormRef.resetFields()
    },
    // 点击按钮，添加新用户
    addStaff() {
      this.$refs.addFormRef.validate(async valid => {
        if (!valid) return;
     this.axios.post(
          "addStaff",   this.qs.stringify({
               staffName:this.addForm.staffName,
              staffSex:this.addForm.staffSex,
               staffAge:this.addForm.staffAge,
               staffPhone:this.addForm.staffPhone,
               staffAccountNum:this.addForm.staffAccountNum,//银行账户
               staffAccountMoney:this.addForm.staffAccountMoney,//账户余额
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
             this.$Message.info("添加失败");
              console.log(err.response);
            }
          }.bind(this)
        );
      })
    },
      showEditDialog(id,name,sex,age,phone,account,money){
     console.log("showid:"+id+name+sex+age+phone+account+money);

      this.editForm.staffId=id;
      this.editForm.staffName=name;
      this.editForm.staffSex=sex;
      this.editForm.staffAge=age;
       this.editForm.staffPhone=phone;
      this.editForm.staffAccountNum=account;
      this.editForm.staffAccountMoney=money;
      this.editDialogVisible = true
      },
// 监听修改用户对话框的关闭事件
    editDialogClosed() {
      this.$refs.editFormRef.resetFields();
    },
    // 修改用户信息并提交
    editUserInfo() {
      this.$refs.editFormRef.validate( valid => {
        if (!valid) return;
          this.axios.post(
          "updateStaff",   this.qs.stringify({
            staffId:this.editForm.staffId,
               staffName:this.editForm.staffName,
              staffSex:this.editForm.staffSex,
               staffAge:this.editForm.staffAge,
               staffPhone:this.editForm.staffPhone,
               staffAccountNum:this.editForm.staffAccountNum,//银行账户
               staffAccountMoney:this.editForm.staffAccountMoney,//账户余额
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data);
            this.editDialogVisible=false;
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
          "deleteStaffById",  this.qs.stringify({
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
