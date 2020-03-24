<template>
  <div>
 
    <div id="chart_example"></div>

  </div>
</template>
 
<script>
import echarts from 'echarts'
  export default {
    data() {
      return {
          
   option1 :{
        backgroundColor: "#344b58",
      "title": {
        "text": "员工一年平均薪资可视化",
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
       var that = this;
     setTimeout(function(){
                 that.doSearch();
      },3000);
    },
    methods: {
      doSearch(){
          this.axios.post(
          "findAverageSalaryByMonth", this.qs.stringify({
            num:12,
            code:this.value,
            endTime:this.query
          })
        )
        .then(
          function(res) {
            if(res.data.code==200)
           {
            console.log(res.data.data);
                var mapNum=res.data.data;
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
      }
    },
    watch: {},
    created() {
 
    }
  }
</script>
<style scoped>
 
  #chart_example{
    width: 100%;
    height: 500px;
    border: 1px solid red;
    margin: 0 auto;
      border-radius: 25px;
  }
 
</style>