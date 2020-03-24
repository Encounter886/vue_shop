<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/mao.jpg" style="margin-left:20px; width:45px;height :40px;border-radius: 25px" alt="">
        <span>自助餐厅管理及数据可视化分析综合系统</span>
      </div>
      <el-button type="info" @click="logout" style="margin-right:10px;">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '45px' : '200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu background-color="#1F186B" text-color="#fff" active-text-color="#409EFF" unique-opened :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">
          <!-- 一级菜单 -->

<!-- 菜品管理 -->
<el-submenu index="1" >
        <template slot="title">
          <i class="iconfont icon-tijikongjian"></i>菜品管理 </template>
          <el-menu-item index="showFoodCa">
            <template slot="title"> <i class="iconfont icon-danju"></i>菜品种类 </template>
          </el-menu-item>
            
      <el-menu-item index="inFood">
            <template slot="title"> <i class="iconfont icon-danju"></i>入库查询</template>
          </el-menu-item>
          <el-menu-item index="outFood">
            <template slot="title"> <i class="iconfont icon-danju"></i>出库查询</template>
          </el-menu-item>
      </el-submenu>

<!-- 顾客收银模块 -->
 <el-submenu index="2">
        <template slot="title">
          <i class="iconfont icon-user"></i>顾客收银 </template>
          <el-menu-item index="waitNum">
            <template slot="title"> <i class="iconfont icon-danju"></i>顾客排号叫号 </template>
          </el-menu-item>
            <el-menu-item index="pay">
            <template slot="title"> <i class="iconfont icon-danju"></i>顾客支付</template>
          </el-menu-item>
           </el-menu-item>
            <el-menu-item index="selectPay">
            <template slot="title"> <i class="iconfont icon-danju"></i>顾客消费记录查询</template>
          </el-menu-item>
      </el-submenu>

<!-- 员工管理模块 -->
  <el-submenu index="3" >
        <template slot="title">
          <i class="iconfont icon-user"></i>员工管理 </template>
          <el-menu-item index="staffInfo">
            <template slot="title"> <i class="iconfont icon-danju"></i>员工信息管理 </template>
          </el-menu-item>
            <el-menu-item index="salaryInfo">
            <template slot="title"> <i class="iconfont icon-danju"></i>员工工资发放</template>
          </el-menu-item>
           </el-menu-item>
            <el-menu-item index="salaryFind">
            <template slot="title"> <i class="iconfont icon-danju"></i>员工工资查询</template>
          </el-menu-item>
      </el-submenu>

<!-- 数据可视化模块 -->
        <el-submenu index="4" >
        <template slot="title">
          <i class="iconfont icon-tijikongjian"></i>数据可视化
          </template>
         </el-menu-item>
          <el-menu-item index="foodOutVisual"> <i class="iconfont icon-danju"></i>菜品消耗折线图</el-menu-item>
           <el-menu-item index="foodOutPie"> <i class="iconfont icon-danju"></i>菜品消耗饼图</el-menu-item>
          <el-menu-item index="salaryVisual"> <i class="iconfont icon-danju"></i>员工薪资可视化</el-menu-item>
      </el-submenu>

      <!-- j监控安全 -->
          <el-submenu index="5" >
        <template slot="title">
          <i class="iconfont icon-tijikongjian"></i>监控安全
          </template>
         </el-menu-item>
          <el-menu-item index="localMonitor"> <i class="iconfont icon-danju"></i>本地相机组</el-menu-item>
           <el-menu-item index="pcMonitor"> <i class="iconfont icon-danju"></i>pc相机组</el-menu-item>
          <el-menu-item index="moveMonitor"> <i class="iconfont icon-danju"></i>移动相机组</el-menu-item>
      </el-submenu>

        </el-menu>
      </el-aside>


      <!-- 右侧内容主体 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      // 左侧菜单数据
      menulist: [],
      iconsObj: {
        '1': 'iconfont icon-user',
        '2': 'iconfont icon-tijikongjian',
        '3': 'iconfont icon-shangpin',
        '4': 'iconfont icon-danju',
        '5': 'iconfont icon-baobiao'
      },
      // 是否折叠
      isCollapse: false,
      activePath:'',
    }
  },
  created() {
     //this.activePath = window.sessionStorage.getItem('activePath')

  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push('/login')
    },
    
    // 点击按钮，切换菜单的折叠与展开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
  
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #201f86;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    span {
      margin-left: 15px;
    }
  }
}
.el-button--info {
    color: #FFF;
    background-color: #DB7F3B;
    border-color: red;
}
.el-aside {
  background-color:rgb(20, 26, 72);
  .el-menu {
    border-right: none;
  }
}

.el-main {
  background-color: #eaedf1;
}

.iconfont {
  margin-right: 10px;
}

.toggle-button {
  background-color: rgb(53, 31, 114);
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
