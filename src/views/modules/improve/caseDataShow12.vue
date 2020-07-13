<template>
  <div id = "databinding">
    <button v-on:click = "show = !show">点我</button>
    <transition
      v-on:before-enter="beforeEnter"
      v-on:enter="enter"
      v-on:leave="leave"
      v-bind:css="false"
    >
      <p v-if="show">菜鸟教程 -- 学的不仅是技术，更是梦想！！！</p>
    </transition>
  </div>
</template>

<script>
  import Velocity from 'velocity-animate'
  export default {
    data () {
      return {
        el: '#databinding',
        dataForm: {
          lxNum: 797,
          ztNum: 797,
          xxsAll: 0,
          xxsJion: 0,
          xxsJg: 0,
          gxwAll: 0,
          gxwJion: 0,
          gxwJg: 0,
          ypAll: 0,
          ypJion: 0,
          ypJg: 0,
          wqjAll: 0,
          wqjJion: 0,
          wqjJg: 0,
          zyAll: 0,
          zyJion: 0,
          zyJg: 0,
          dhjAll: 0,
          dhjJion: 0,
          dhjJg: 0,
          nearpassRate: [],
          nearparticipationRate: [],
          passRate: [],
          participationRate: [],
          keyTime1: '',
          keyTime2: '',
          keyTime3: '',
          key: '',
          jobNo: '451692'
        },
        show: false,
        exportList: [],
        nearDatalistJ: [81, 71, 90, 70, 80, 60],
        nearDatalistC: [90, 90, 90, 90, 90, 90],
        hisDatalistJ: [],
        hisDatalistC: [],
        colors: [{color: 'rgb(46,199,201)'}, {color: 'rgb(182,162,222)'}, {color: 'rgb(90,177,239)'}, {color: 'rgb(255,185,128)'}, {color: 'rgb(51,132,123)'}],
        pieChart1: [],
        pieChart2: [],
        nameList: ['PVD厂', '郭祥伟', '岳平', '王启军', '周颖', '邓海军'],
        dateListTime: [],
        fileUploadBtnText: '导入数据',
        dataList: [],
        pageIndex: 1,
        pageSize: 5,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false,
        myChart: '',
        myChart1: '',
        myChart2: '',
        myChart3: '',
        myChart4: '',
        myChart5: '',
        myChart6: ''
      }
    },
    components: {
    },
    activated () {
      this.getDataList()
    },
    mounted () {
      this.drawLine()
    },
    methods: {
      beforeEnter: function (el) {
        el.style.opacity = 0
        el.style.transformOrigin = 'left'
      },
      enter: function (el, done) {
        Velocity(el, { opacity: 1, fontSize: '1.4em' }, { duration: 300 })
        Velocity(el, { fontSize: '1em' }, { complete: done })
      },
      leave: function (el, done) {
        Velocity(el, { rotateZ: '10deg' })
        Velocity(el, { rotateZ: '200deg' })

      }
    }
  }
</script>
<style>
  #login {
    /*background-color: rgba(239,241,244,1);*/
    /*height: 85vh;*/
    /*width: 100vw;*/
  }
  .cardEl {
    transition: all .5s;
    padding: 0;
    background-color:rgba(245,248,253,1)
  }
  .cardEl:hover{
    margin-top: -10px;
    background: RGB(49,199,201);
  }
  .homeP{
    font-family: "宋体","仿宋",sans-serif;/*若电脑不支持宋体，则用仿宋，若不支持仿宋，则在sans-serif中找*/
    font-weight: bold;
    position: absolute;
    left: 0;
    font-size: 80%;
    /*font-style: italic;*/
    color: RGB(54, 163, 247);/*字体颜色*/
    opacity: 1;/*字体的透明度：1：默认样式，0：全透明*/
  }
  .homePVD{
    font-family: "宋体","仿宋",sans-serif;/*若电脑不支持宋体，则用仿宋，若不支持仿宋，则在sans-serif中找*/
    font-weight: bold;
    position: absolute;
    left: 0;
    font-size: 150%;
    /*font-style: normal;*/
    color: RGB(54, 163, 247);/*字体颜色*/
    opacity: 1;/*字体的透明度：1：默认样式，0：全透明*/
  }
  .homespan{
    font-family: "宋体","仿宋",sans-serif;/*若电脑不支持宋体，则用仿宋，若不支持仿宋，则在sans-serif中找*/
    font-weight: bold;
    font-size: 100%;
    /*font-style: italic;*/
    color: RGB(140,140,140);/*字体颜色*/
    opacity: 1;/*字体的透明度：1：默认样式，0：全透明*/
  }
  .homevalue{
    font-family: "宋体","仿宋",sans-serif;/*若电脑不支持宋体，则用仿宋，若不支持仿宋，则在sans-serif中找*/
    font-weight: bold;
    font-size: 120%;
    color: RGB(102,102,102);/*字体颜色*/
    opacity: 1;/*字体的透明度：1：默认样式，0：全透明*/
  }
  .ptitle{
    font-size: 150%;
    font-family: '宋体';
    color: #aa1111;
    font-style: italic;
    opacity: 0.5;
    font-weight: bold;
    position:relative;
    text-align:center;
    margin:0;
  }
  .homeimg {
    width: 36px;
    height: auto;
    margin-right: 5px;
    border-radius: 100%;
    vertical-align: middle;
  }
  .paddingS{
    margin-top: 10px;
  }
</style>
