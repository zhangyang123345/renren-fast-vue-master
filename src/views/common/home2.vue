<template>

<!--<div style="width: 100%;height: 100%">-->
      <!--<div id="myChart" class="mod-home" style="width: 100%;height: 100%">-->
      <!--</div>-->
<!--</div>-->
  <div class="block">
    <el-row type="flex"  style="margin-top:20px"  class="el-row"  justify="center">
      <el-col :span="10"><div :style="'height:'+fullHeight*0.25+'px'"   id="myChart1"  class="grid-content"></div>
      </el-col>
      <el-col :span="4">
          <span @click="allGet()" class=" title">
            仓库管理数据展示
        </span>
        <div class=" title2">
            (负责人：XX)
        </div>
      </el-col>
      <el-col :span="10"><div :style="'height:'+fullHeight*0.25+'px'" id="myChart2"  class="grid-content"></div>
      </el-col>
    </el-row>
    <el-row type="flex" style="margin-top:-80px"  class="el-row"  justify="center">
      <el-col :span="4">
        <div style="margin-top: 80px">
          <div align="center">
        <span class=" title1">
            当月费用排名前五
        </span>
          </div>
          <table style="border-collapse: collapse;margin: 0 auto;background: #93B742">
            <tr>
              <th style="width: 10%;text-align: center" >排名</th>
              <th  style="width: 80%;text-align: center">名称</th>
              <th  style="width: 10%;text-align: center">费用</th>
            </tr>
            <tr>
              <th  style="text-align: center">1</th>
              <td id="wp1" style="text-align: center">物品1</td>
              <td id="sl1" style="text-align: center">1200</td>
            </tr>
            <tr>
              <th  style="text-align: center">2</th>
              <td id="wp2" style="text-align: center">物品2</td>
              <td id="sl2" style="text-align: center">1000</td>
            </tr>
            <tr>
              <th  style="text-align: center">3</th>
              <td id="wp3" style="text-align: center">物品3</td>
              <td id="sl3" style="text-align: center">330</td>
            </tr>
            <tr>
              <th  style="text-align: center">4</th>
              <td id="wp4" style="text-align: center">物品4</td>
              <td id="sl4" style="text-align: center">130</td>
            </tr>
            <tr>
              <th  style="text-align: center">5</th>
              <td id="wp5" style="text-align: center">物品5</td>
              <td id="sl5" style="text-align: center">30</td>
            </tr>
          </table>
          <!--<h3>项目介绍</h3>-->
          <!--<li style="float: left">排名</li> <div>12</div>-->
          <!--<div>13</div>-->
          <!--<div>13</div>-->
        </div>
      </el-col>
      <el-col :span="16"><div :style="'height:'+fullHeight*0.6+'px'" id="myChart"  class="grid-content"></div>
      </el-col>
      <el-col :span="4">
        <div style="margin-top: 80px">
          <div align="center">
        <span class=" title1">
            当月报废排名前五
        </span>
          </div>
          <table style="border-collapse: collapse;margin: 0 auto;background: #d44239">
            <tr>
              <th style="width: 10%;text-align: center" >排名</th>
              <th  style="width: 80%;text-align: center">名称</th>
              <th  style="width: 10%;text-align: center">数量</th>
            </tr>
            <tr>
              <th  style="text-align: center">1</th>
              <td id="wp11" style="text-align: center">物品5</td>
              <td id="sl11" style="text-align: center">30</td>
            </tr>
            <tr>
              <th  style="text-align: center">2</th>
              <td id="wp12" style="text-align: center">物品7</td>
              <td id="sl12" style="text-align: center">10</td>
            </tr>
            <tr>
              <th  style="text-align: center">3</th>
              <td id="wp13" style="text-align: center">物品6</td>
              <td id="sl13" style="text-align: center">10</td>
            </tr>
            <tr>
              <th  style="text-align: center">4</th>
              <td id="wp14" style="text-align: center">物品4</td>
              <td id="sl14" style="text-align: center">6</td>
            </tr>
            <tr>
              <th  style="text-align: center">5</th>
              <td id="wp15" style="text-align: center">物品8</td>
              <td id="sl15" style="text-align: center">5</td>
            </tr>
          </table>
          <!--<h3>项目介绍</h3>-->
          <!--<li style="float: left">排名</li> <div>12</div>-->
          <!--<div>13</div>-->
          <!--<div>13</div>-->
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" style="margin-top:-80px"  class="el-row"  justify="center">
      <el-col :span="10"><div id="myChart3"  :style="'height:'+fullHeight*0.25+'px'" class="grid-content"></div>
      </el-col>
      <el-col :span="4">
      </el-col>
      <el-col :span="10"><div :style="'height:'+fullHeight*0.25+'px'" id="myChart4"  class="grid-content"></div>
      </el-col>
    </el-row>
    <!--<div id="myChart" style="width: 30%;height: 100%;float: left">-->
      <!--</div>-->
    <!--<div id="myChart1" style="width: 30%;height: 100%;float: left">-->
    <!--</div>-->
  </div>
</template>
<script>
    export default {
      name: '',
      data () {
        return {
          backgroundDiv: {
            backgroundImage: 'url(' + require('../../assets/img/46.jpg') + ')',
            backgroundRepeat: 'no-repeat',
            backgroundSize: '100% 100%',
            height: document.documentElement.clientHeight + 'px'
          },
          msg: '仓库储存量',
          fullHeight: document.documentElement.clientHeight,
          storages: [73489, 89034, 104970, 131744, 90230], //库存量
          usage: [2348, 9034, 10497, 31744, 30230], //使用量
          userRank: [], //使用前五
          dumpingRank: [], //报废排名
          stores: ['仓库1', '仓库2', '仓库3', '仓库4', '仓库5'], //仓库
          storesAndNum: [
            {value: 73489, name: '仓库1'},
            {value: 89034, name: '仓库2'},
            {value: 104970, name: '仓库3'},
            {value: 131744, name: '仓库4'},
            {value: 90230, name: '仓库5'}
          ], //仓库&存量
          goods: [
            // {value: 23489, name: '物品1', selected: true},
            {value: 9034, name: '物品1'},
            {value: 10470, name: '物品2'},
            {value: 13174, name: '物品3'},
            {value: 3230, name: '物品4'},
            {value: 4604, name: '物品5'},
            {value: 1130, name: '物品6'},
            {value: 78210, name: '物品7'}
          ], // 物品
          putinge: [238, 9034, 1049, 31744, 60230], //进货量
          dumpingge: [8, 4, 10, 31, 6] //报废量
        }
      },
      mounted () {
        this.get_bodyHeight()
          // const that = this
          // window.onresize = () => {
          //   return (() => {
          //     window.screenHeight = document.body.clientHeight
          //   that.screenHeight = window.screenHeight
          // })()
          // }
        this.drawLine()
      },
      watch: {
        fullHeight (val) {//监控浏览器高度变化
          if(!this.timer) {
            this.fullHeight = val
            this.timer = true
            let that = this
            setTimeout(function (){
              that.timer = false
            },400)
          }

        }
      },
      methods: {
        get_bodyHeight () {//动态获取浏览器高度
          const that = this
          window.onresize = () => {
            return (() => {
              window.fullHeight = document.documentElement.clientHeight
            that.fullHeight = window.fullHeight
          })()
          }
        },
        drawLine () {

          // 基于准备好的dom，初始化echarts实例
          let myChart1 = this.$echarts.init(document.getElementById('myChart1'))
          // 绘制图表
          myChart1.setOption({
            color: ['#9BC878'],
            title: {
              text: '仓库储存量',
              // color: '#4b67c2',
              textStyle: {//主标题文本样式{"fontSize": 18,"fontWeight": "bolder","color": "#333"}
                // color: "#c2bab9"
              }
            },
            tooltip: {
              trigger: 'axis',
              axisPointer: {
                type: 'shadow'
              }
            },
            legend: {
              show: false,
              data: ['2011年']
            },
            grid: {
              top: '20%',
              left: '3%',
              right: '10%',
              bottom: '3%',
              containLabel: true
            },
            xAxis: {
              type: 'value',
              boundaryGap: [0, 0.01],
              axisLine: {
                lineStyle: {
                  // color: '#bdc2af'
                }}
            },
            yAxis: {
              type: 'category',
              data: this.stores,
              axisLine: {
                lineStyle: {
                  // color: '#bdc2af'
              }}
            },
            series: [
              {
                // name: '2011年',
                type: 'bar',
                data: this.storages,
                itemStyle: {
                  normal: {
                    label: {
                      show: true, //开启显示
                      position: 'right', //在上方显示
                      textStyle: { //数值样式
                        color: 'black',
                        fontSize: 16
                      }
                    }
                  }
                }
              }
            ]
          })
          // 基于准备好的dom，初始化echarts实例
          let myChart = this.$echarts.init(document.getElementById('myChart'))
          // 绘制图表
          myChart.setOption({
            tooltip: {
              trigger: 'item',
              formatter: '{a} <br/>{b}: {c} ({d}%)'
            },
            legend: {
              show: false,
              orient: 'vertical'
              // left: 600,
              // top: 600,
              // data: ['total']
            },
            series: [
              {
                name: '存量',
                type: 'pie',
                selectedMode: 'single',
                radius: [0, '15%'],

                label: {
                  position: 'inner'
                },
                labelLine: {
                  show: false
                },
                data: this.storesAndNum
              },
              {
                name: '存量与占比',
                type: 'pie',
                radius: ['30%', '50%'],
                label: {
                  formatter: '{a|{a}}{abg|}\n{hr|}\n  {b|{b}：}{c}  {per|{d}%}  ',
                  backgroundColor: '#eee',
                  borderColor: '#aaa',
                  borderWidth: 1,
                  borderRadius: 4,
                  // shadowBlur:3,
                  // shadowOffsetX: 2,
                  // shadowOffsetY: 2,
                  // shadowColor: '#999',
                  // padding: [0, 7],
                  rich: {
                    a: {
                      color: '#999',
                      lineHeight: 12,
                      align: 'center'
                    },
                    // abg: {
                    //     backgroundColor: '#333',
                    //     width: '100%',
                    //     align: 'right',
                    //     height: 22,
                    //     borderRadius: [4, 4, 0, 0]
                    // },
                    hr: {
                      borderColor: '#aaa',
                      width: '100%',
                      borderWidth: 0.5,
                      height: 0
                    },
                    b: {
                      fontSize: 10,
                      lineHeight: 23
                    },
                    per: {
                      color: '#eee',
                      backgroundColor: '#334455',
                      padding: [2, 4],
                      borderRadius: 2
                    }
                  }
                },
                data: this.goods
              }
            ]
          })
          // 基于准备好的dom，初始化echarts实例
          let myChart2 = this.$echarts.init(document.getElementById('myChart2'))
          // 绘制图表
          myChart2.setOption({
            color: ['#9BC878'],
              title: {
                text: '一周各仓库使用量'
              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                show: false,
                data: ['2011年']
              },
              grid: {
                top: '20%',
                left: '3%',
                right: '10%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: {
                type: 'value',
                boundaryGap: [0, 0.01]
              },
              yAxis: {
                type: 'category',
                data: this.stores
              },
              series: [
                {
                  // name: '2011年',
                  type: 'bar',
                  data: this.usage,
                  itemStyle: {
                    normal: {
                      label: {
                        show: true, //开启显示
                        position: 'right', //在上方显示
                        textStyle: { //数值样式
                          color: 'black',
                          fontSize: 16
                        }
                      }
                    }
                  }
                }
              ]
          })
          let myChart3 = this.$echarts.init(document.getElementById('myChart3'))
          // 绘制图表
          myChart3.setOption({
            color: ['#9BC878'],
            title: {
              text: '一周各仓库进货量'
            },
            tooltip: {
              trigger: 'axis',
              axisPointer: {
                type: 'shadow'
              }
            },
            legend: {
              show: false,
              data: ['2011年']
            },
            grid: {
              top: '20%',
              left: '3%',
              right: '10%',
              bottom: '3%',
              containLabel: true
            },
            xAxis: {
              type: 'value',
              boundaryGap: [0, 0.01]
            },
            yAxis: {
              type: 'category',
              data: this.stores
            },
            series: [
              {
                // name: '2011年',
                type: 'bar',
                data: this.putinge,
                itemStyle: {
                  normal: {
                    label: {
                      show: true, //开启显示
                      position: 'right', //在上方显示
                      textStyle: { //数值样式
                        color: 'black',
                        fontSize: 16
                      }
                    }
                  }
                }
              }
            ]
          })
          let myChart4 = this.$echarts.init(document.getElementById('myChart4'))
          // 绘制图表
          myChart4.setOption({
            title: {
              text: '一周各仓库报废量'
            },
            tooltip: {
              trigger: 'axis',
              axisPointer: {
                type: 'shadow'
              }
            },
            legend: {
              show: false,
              data: ['2011年']
            },
            grid: {
              top: '20%',
              left: '3%',
              right: '10%',
              bottom: '3%',
              containLabel: true
            },
            xAxis: {
              type: 'value',
              boundaryGap: [0, 0.01]
            },
            yAxis: {
              type: 'category',
              data: this.stores
            },
            series: [
              {
                // name: '2011年',
                type: 'bar',
                data: this.dumpingge,
                itemStyle: {
                  normal: {
                    label: {
                      show: true, //开启显示
                      position: 'right', //在上方显示
                      textStyle: { //数值样式
                        color: 'black',
                        fontSize: 16
                      }
                    }
                  }
                }
              }
            ]
          })
          myChart.getZr().on('click', function (params) {
            //0里面的图形点击事件,1外面图形点击事件
            if (params.target.seriesIndex == 0) {
            } else {
              this.storages = [7489, 8034, 1970, 3744, 9030]
              this.usage = [238, 934, 1097, 344, 3020]
              this.putinge = [28, 934, 109, 3144, 6230]
              this.dumpingge = [2, 0, 1, 3, 6]
              var option = {
                series : [ {
                  data : this.storages
                } ]
              };
              var option1 = {
                series : [ {
                  data : this.usage
                } ]
              };
              var option2 = {
                series : [ {
                  data : this.putinge
                } ]
              };
              var option3 = {
                series : [ {
                  data : this.dumpingge
                } ]
              };
              myChart1.setOption(option)
              myChart2.setOption(option1)
              myChart3.setOption(option2)
              myChart4.setOption(option3)
            }
            // var name = params.target.dataIndex;
          })
        },
     allGet () {
          this.storages = [73489, 89034, 104970, 131744, 90230]
          this.usage = [2348, 9034, 10497, 31744, 30230]
          this.putinge = [238, 9034, 1049, 31744, 60230]
          this.dumpingge = [8, 4, 10, 31, 6]
          var option = {
            series : [ {
              data : this.storages
            } ]
          };
          var option1 = {
            series : [ {
              data : this.usage
            } ]
          };
          var option2 = {
            series : [ {
              data : this.putinge
            } ]
          };
          var option3 = {
            series : [ {
              data : this.dumpingge
            } ]
          };
       this.$echarts.init(document.getElementById('myChart1')).setOption(option)
       this.$echarts.init(document.getElementById('myChart2')).setOption(option1)
       this.$echarts.init(document.getElementById('myChart3')).setOption(option2)
       this.$echarts.init(document.getElementById('myChart4')).setOption(option3)
        }
      }
    }
</script>

<style>
  .title {
    font-size: 20px;
    font-weight: bold;
    color: #667AFA;
  }
  .title1 {
    font-size: 18px;
    font-weight: bold;
  }
  .title2 {
    font-size: 18px;
    font-weight: bold;
    margin-left: 25px;
  }
  .block{
    padding: 30px 24px;
    background-color: rgba(194, 186, 185, 0.4);
  }

  .grid-content {
    border-radius: 4px;
  }
  table, table tr th, table tr td {
    border: 1px solid #000000;
    font-size: 14px;
    height: 25px;
  }

  table tr td input {
    -moz-appearance: textfield;
    width: 95%;
    height: 95%;
    border: none;
    outline: medium;
    text-align: right;
    background: #FFFFFF;
  }
  .el-row {
    display: flex;
    flex-wrap: wrap;
  }

</style>


