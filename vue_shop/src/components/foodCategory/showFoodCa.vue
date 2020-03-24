<template>
  <div>
    <!-- 卡片视图区域 -->
    <el-card>
      <!-- 搜索与添加区域 -->
      <el-row :gutter="20">
        <el-col :span="8">
      <el-button type="primary" @click="findAddFood">查询所有菜品</el-button>
        </el-col>
        <el-col :span="4">
          <el-button type="primary" @click="addDialogVisible = true">添加菜品</el-button>
        </el-col>
      </el-row>
 <!--foodId  foodName  foodPrice foodType;//菜品种类 1：蔬菜类   2：肉类  3 ：豆制品 4：水果   5；面食 -->
      <!-- 用户列表区域 -->
      <el-table :data="foodlist" border stripe height="400">
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column label="菜品id" prop="foodId"></el-table-column>
        <el-table-column label="菜品名称" prop="foodName"></el-table-column>
        <el-table-column label="菜品价格(单位：元)" prop="foodPrice"></el-table-column>
        <el-table-column label="菜品类型" prop="foodType"></el-table-column>

        <el-table-column label="操作" width="180px">
          <template slot-scope="scope">
            <!-- 修改按钮 -->
            <el-button type="primary" icon="el-icon-edit" size="mini" @click="showEditDialog(scope.row.foodId,scope.row.foodName,scope.row.foodPrice,scope.row.foodType)"></el-button>
            <!-- 删除按钮 -->
            <el-button type="danger" icon="el-icon-delete" size="mini" @click="removeById(scope.row.foodId)"></el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>

 <!-- 添加caipin的对话框 -->
    <el-dialog title="添加用户" :visible.sync="addDialogVisible" width="50%" @close="addDialogClosed">
      <!-- 内容主体区域 -->
      <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="70px">
        <el-form-item label="id" prop="id">
          <el-input v-model="addForm.id" disabled></el-input>
        </el-form-item>
        <el-form-item label="菜名" prop="name">
          <el-input v-model="addForm.name"></el-input>
        </el-form-item>
        <el-form-item label="价格" prop="price">
          <el-input v-model="addForm.price"></el-input>
        </el-form-item>
        <el-form-item label="类型" prop="type">
          <el-input v-model="addForm.type" ></el-input>
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
            <el-form-item label="菜品id">
          <el-input v-model="editForm.id" disabled></el-input>
        </el-form-item>
        <el-form-item label="菜品名">
          <el-input v-model="editForm.name" disabled></el-input>
        </el-form-item>
        <el-form-item label="菜品类型" >
          <el-input v-model="editForm.type" disabled></el-input>
        </el-form-item>
        <el-form-item label="菜品价格" prop="price">
          <el-input v-model="editForm.price"></el-input>
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

      // 控制添加对话框的显示与隐藏
     addDialogVisible:false,
     addForm:{
                  id:'',
                  name:'',
                  price:'',
                  type:''
     },
     addFormRules:{},
// 控制修改用户对话框的显示与隐藏
      editDialogVisible: false,
    
       // 控制修改用户对话框的显示与隐藏
      editDialogVisible: false,
      // 查询到的用户信息对象
      editForm: {id:'',
                  name:'',
                  price:'',
                  type:''
       },
      // 修改表单的验证规则对象
      editFormRules: {
      },

      }
    }
  ,
  created() {},
  methods: {
     async findAddFood(){
    
             this.axios.post(
          "findAllVegetable", this.qs.stringify({
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
        if (!valid) return
   
            this.axios.post(
          "updateOrInsertVegetable",   this.qs.stringify({
            foodName: this.addForm.name,
             foodPrice: this.addForm.price,
              foodType: this.addForm.type,
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
      showEditDialog(id,name,price,type){
    

      this.editForm.id=id;
      this.editForm.name=name;
      this.editForm.price=price;
      this.editForm.type=type;
      this.editDialogVisible = true;
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
          "updateVegetable1",  this.qs.stringify({
            foodId:this.editForm.id,
            foodName: this.editForm.name,
             foodPrice: this.editForm.price,
              foodType: this.editForm.type,
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
      this.editDialogVisible = false;
        // 提示修改成功
        this.$message.success('更新用户信息成功！');
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
          "deleteVegetableById",  this.qs.stringify({
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
