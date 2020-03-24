<template>
 <div id="donghua">
   <div>
    <div class="login-container" style="background-color: #141a48;margin: 0px;overflow: hidden;">
    <div id="canvascontainer" ref="can"> </div>
  <div class="login-title">自助餐厅管理及数据可视化分析综合系统</div>
    <Form
      ref="loginForm"
      autocomplete="on"
      :model="loginForm"
      :rules="loginRules"
      class="card-box login-form"
    >
      <Form-item prop="email">
        <Input type="text" v-model="loginForm.email"  prefix-icon="iconfont icon-3702mima" placeholder="Username" autocomplete="on">
          <Icon type="ios-person-outline" slot="prepend"></Icon>
        </Input>
      </Form-item>
   
       <FormItem prop="password">
          <Input type="password" v-model="loginForm.password" placeholder="Password" 
            @keyup.enter.native="handleLogin">
            <Icon type="ios-lock-outline" slot="prepend"></Icon>
          </Input>
        </FormItem>
      <Form-item>
        <Button type="primary" @click="handleLogin('loginForm')" long>登录</Button>
      </Form-item>
     
    </Form>
  </div>
  </div>
 </div>
</template>

<script>


export default {
   name:"login",
  data() {
    const validateEmail = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error("请输入正确的合法用户名，不小于6位"));
      } else {
        callback();
      }
    };
    const validatePass = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error("密码不能小于6位"));
      } else {
        callback();
      }
    };
    return {
      loginForm: {
        email: "zhouzixin",
        password: "123456"
      },
      loginRules: {
        email: [{ required: true, trigger: "blur", validator: validateEmail }],
        password: [{ required: true, trigger: "blur", validator: validatePass }]
      },
      loading: false,
      showDialog: false
    };
  }, 
  created(){

  },
   mounted() {
  },
  methods: {
   
    handleLogin() {
  
      this.$refs.loginForm.validate(valid => {
        if (valid) {
              // this.loading = false;
 // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
        //   1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
        //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
      //  window.sessionStorage.setItem('token', res.data.token)
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
         this.loading = true;
             this.axios.post(
          "loginByUserName",
          this.qs.stringify({
            username: this.loginForm.email,
            password: this.loginForm.password
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data);
           this.$Message.success("登录成功");                
              this.loading = false;
             window.sessionStorage.setItem('token', res.headers.authorization);
            this.$router.push('/home');
           }else{
            console.log(res.data);
                 alert("登陆失败");
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

        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};

 
</script>
<style lang="less" scoped>
.login-title{
  position: relative;
font-size:  40px;
text-align: center;
color: #2d8cf0;
top: 80px;
}
.login-container a {
  color: #0078de;
}
#canvascontainer {
  position: absolute;
  top: 0px;
}
.wz-input-group-prepend {
  background-color: #141a48;
  border: 1px solid #2d8cf0;
  border-right: none;
  color: #2d8cf0;
}
</style>

<style rel="stylesheet/scss" lang="scss">
.tips {
  font-size: 14px;
  color: #fff;
  margin-bottom: 5px;
}
.login-container {
  height: 100vh;
  background-color: #2d3a4b;

  input:-webkit-autofill {
    box-shadow: 0;
    -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
    -webkit-text-fill-color: #fff !important;
  }
  input {
    background: transparent;
    border: 1px solid #2d8cf0;
    -webkit-appearance: none;
    border-radius: 3px;
    padding: 12px 5px 12px 15px;
    color: #eeeeee;
    height: 47px;
  }
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;
  }
  .svg-container {
    padding: 6px 5px 6px 15px;
    color: #889aa4;
  }

  .title {
    font-size: 26px;
    font-weight: 400;
    color: #eeeeee;
    margin: 0px auto 40px auto;
    text-align: center;
    font-weight: bold;
  }

  .login-form {
    position: absolute;
    left: 0;
    right: 0;
    width: 400px;
    padding: 35px 35px 15px 35px;
    margin: 120px auto;
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }

  .forget-pwd {
    color: #fff;
  }
}
</style>
