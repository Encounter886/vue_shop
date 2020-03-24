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
         <!-- <Button type="primary" size="large" @click="exportData(1)"><Icon type="ios-download-outline"></Icon> 导出原始数据</Button>
    <Button type="primary" size="large" @click="exportData(2)"><Icon type="ios-download-outline"></Icon> 导出排序和过滤后的数据</Button> -->
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
        "text": "货物消耗数量图",
        x: "2%",

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
        "data": ['女', '男', '平均']
    },
     
    "calculable": true,
        xAxis : [
          {
            type : 'category',
            data : ['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月',],
            axisTick: {
              alignWithLabel: true
            },
              axisLabel: {
                interval:0,
                rotate:45, //代表逆时针旋转45度
            }
          }
        ],
      "yAxis": [{
        "type": "value",
        "splitLine": {
            "show": false
        },
        "axisLine": {
            lineStyle: {
                color: '#90979c'
            }
        },
        "axisTick": {
            "show": false
        },
        "axisLabel": {
            "interval": 0,

        },
        "splitArea": {
            "show": false
        },

    }],
        series : [
          {
            name:'数量',
            type:'bar',
           "barMaxWidth": 35,
            "barGap": "10%",
            "itemStyle": {
                "normal": {
                    "color": "rgba(255,144,128,1)",
                    "label": {
                        "show": true,
                        "textStyle": {
                            "color": "#fff"
                        },
                        "position": "insideTop",
                     
                    }
                }
            },
            data:[995,666,444,858,654,236,645,546,846,225,547,356]
          },
             {
           "name": "",
            "type": "line",
            symbolSize:10,
            symbol:'circle',
            "itemStyle": {
                "normal": {
                    "color": "rgba(252,230,48,1)",
                    "barBorderRadius": 0,
                    "label": {
                        "show": true,
                        "position": "top",
                        formatter: function(p) {
                            return p.value > 0 ? (p.value) : '';
                        }
                    }
                }
            },
            data:[995,666,444,858,654,236,645,546,846,225,547,356]
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
                 var mapPro=res.data.data[1];
                 var keys=[];
                 var nums=[];
                for(var key in mapNum){
                   keys.push(key);
                   nums.push(mapNum[key]);
                }
                 this.option1.xAxis[0].data=keys;
                  this.option1.series[1].data=nums;
                   this.option1.series[0].data=nums;

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
        //  exportData (type) {
        //         if (type === 1) {
        //             this.$refs.table.exportCsv({
        //                 filename: '原始数据',
        //                 columns: this.columns7,
        //                 data: this.video_list
        //             });
        //         } else if (type === 2) {
        //             this.$refs.table.exportCsv({
        //                 filename: '排序和过滤后的数据',
        //                 original: false
        //             });
        //         } 
        //     } 
  
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