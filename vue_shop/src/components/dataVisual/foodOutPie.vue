<template>
  <div>
 
    <div id="chart_example"></div>

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
          <el-input placeholder="请输入日期" v-model="query" clearable>
            <el-button slot="append" icon="el-icon-search" @click="findBy">查询</el-button>
          </el-input>
        </el-col>
        <el-button type="primary" @click="analyse">数据分析</el-button>
        <span style="color:#D35922"> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{msg}}</span>
        </el-row>
        </el-card>
  </div>
</template>
 
<script>
import echarts from 'echarts'
  export default {
    data() {
      return {
          msg:'返回的分析数据',
           options: [{
          value: '1',
          label: '按照天查询'
        },  {
          value: '2',
          label: '按照周查询'
        }, {
          value: '3',
          label: '按照月份查询'
        }],
          query: '',
        value: '',
   option1 :{
        backgroundColor: "#344b58",
      "title": {
        "text": "货物消耗占比饼图",
        x: "center",

        textStyle: {
            color: '#fff',
            fontSize: '22'
        },
        subtextStyle: {
            color: '#90979c',
            fontSize: '16',

        },
    },
    "tooltip": {
        "trigger": "axis",
        "axisPointer": {
            "type": "shadow",
            textStyle: {
                color: "#fff"
            }

        },
    },
    "grid": {
        "borderWidth": 0,
        "top": 60,
        "bottom": 55,
        textStyle: {
            color: "#fff"
        }
    },
    "legend": {
        x: '4%',
        top: '11%',
        textStyle: {
            color: '#90979c',
        },
         orient: 'vertical',
              bottom: 'bottom',
        // data: ['直接访问','邮件营销','联盟广告','视频广告','搜索引擎']
    },
     
    "calculable": true,
        series : [
          {
            name:'',
            type:'pie',
            radius : '55%',
          center: ['50%', '60%'],
          label:{
              normal: {
                       show:true,
                        // position:'inner',
                        textStyle: {
                       fontWeight: 200,
                      fontSize: 15   //文字的字体大小
                          },
            formatter: '{a}  {b} : {c} ({d}%)',/*饼状图内显示百分比*/   
          }
          },
            data:[
               {value:335, name:'直接访问'},
                      {value:310, name:'邮件营销'},
                      {value:234, name:'联盟广告'},
                      {value:135, name:'视频广告'},
                      {value:1548, name:'搜索引擎'}
                ],
            itemStyle: {
                      emphasis: {
                          shadowBlur: 10,
                          shadowOffsetX: 0,
                          shadowColor: 'rgba(0, 0, 0, 0.5)'
                      }
                  }
          }
        ]
      },


      }
    },
    mounted() {
      let this_ = this;
      let myChart = echarts.init(document.getElementById('chart_example'));
     //请求改变option中的3个data
     //此处应有一个循环迭代的过程
      myChart.setOption(this.option1);
 
      //建议加上以下这一行代码，不加的效果图如下（当浏览器窗口缩小的时候）。超过了div的界限（红色边框）
      window.addEventListener('resize',function() {myChart.resize()});
    },
    methods: {
        //数据分析
        analyse(){
                     this.axios.post(
          "findAnalyzePoplationFood", this.qs.stringify({
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
            var a="最受欢迎菜系：";
            for(var i=0;i<5;i++){
               a=a+res.data.data[i]+"、";
            }
              this.msg=a;
                     let this_ = this;
             let myChart = echarts.init(document.getElementById('chart_example'));
             myChart.setOption(this.option1);
             window.addEventListener('resize',function() {myChart.resize()});
             console.log(keys);
                console.log(nums);
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
          findBy(){
             console.log("value:"+this.value+" query:"+this.query);
             if(this.value==1){//day
                  this.findBy(this.query);
             } else if(this.value==2){//week
                    this.findBy(this.query);
             }
             else if(this.value==3){//month
                this.findBy(this.query);
             }else{
                this.$message.info('未知错误');
                return;
             }
      },
     findBy(time){
      
          this.axios.post(
          "findOutFoodByTimeDayMonthWeekEchats", this.qs.stringify({
            code:this.value,
            endTime:this.query
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                var mapNum=res.data.data[0];
                   var list=[mapNum];
                   console.log(mapNum);//
                  
                for(var key in mapNum){
                  var o={};
                     o.name=key;
                     o.value=mapNum[key];
                     list.push(o);
                }
                   console.log(list);//
                   this.option1.series[0].data=list;

                     let this_ = this;
      let myChart = echarts.init(document.getElementById('chart_example'));
      myChart.setOption(this.option1);
      window.addEventListener('resize',function() {myChart.resize()});
             console.log(keys);
                console.log(nums);
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
   
    },
    watch: {},
    created() {
 
    }
  }
</script>
<style scoped>
  h2{
    text-align: center;
    padding: 30px;
    font-size: 18px;
  }
  #chart_example{
    width: 100%;
    height: 400px;
    border: 1px solid red;
    margin: 0 auto;
      border-radius: 25px;
  }
 
</style>