<template>
  <div class="view">
    <el-row :gutter="24" class="el-row head-div" type="flex">
      <el-col span="12">
        <div class="videoBox">
        </div>
      </el-col>
      <el-col span="12">
        <div class="imageBox">
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="24" class="el-row buttom-div" type="flex">
      <el-col span="24">
        <div class="flowBox">
            <el-card class="boxItme" v-for="item in menuList"
                     :key="item" style="background-color:rgba(245,248,253,0)"
                     :click="openMenu('menuId_'+item.itemID)">
              <el-row :gutter="24" class="el-row" type="flex">
                <el-col :span="24">
                  <svg aria-hidden="true" class="site-sidebar__menu-icon icon-svg" :class="item.sicon"><use xlink:href="" :href="item.icon"></use></svg>
                </el-col>
              </el-row>
              <el-row>
                <el-col :span="24">
                  <p style="position: inherit;" class="homePVD">{{item.name}}</p>
                </el-col>
              </el-row>
            </el-card>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        dataForm: {
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
          keytwo: '',
          key: '',
          jobNo: '451692'
        },
        exportList: [],
        nearDatalistJ: [],
        nearDatalistC: [],
        hisDatalistJ: [],
        hisDatalistC: [],
        nameList: [],
        menuList: [],
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
        myChart1: ''
      }
    },
    components: {},
    activated () {
      this.menuCard()
    },
    mounted () {
    },
    methods: {
      menuCard () {
        this.$http({
          url: this.$http.adornUrl('/sys/menu/nav'),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === 0) {
            for(var item in data.menuList){
              data.menuList[item].sicon = 'icon-svg__'+data.menuList[item].icon
              data.menuList[item].icon = '#icon-'+data.menuList[item].icon
            }
            this.menuList = data.menuList
          }
      }).catch((e) => {
          console.log(`%c${e} 请求菜单列表和权限失败，跳转至登录页！！`, 'color:blue')
        router.push({ name: 'login' })
      })
      },
      openMenu (id) {
        menuRout.set(id,true);
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
  .boxItme .icon-svg{
    font-size:38px;
  }
  .boxItme{
    width:100px;
    height:100px;
    font-size: 9px;
    text-align: center;
    display: inline-block;
    margin: 30px 2px;
  }
  .boxItme .site-sidebar__menu-icon{
    margin-right: 0px;
  }
  .view{
    height:100%;
  }
  .head-div{
    height:70%;
  }
  .buttom-div{
    margin-top:1%;
  }
  .imageBox{
    width:100%;
    height: 600px;
    border: #0000cc solid  1px;
  }
  .videoBox{
    width:100%;
    height: 600px;
    border: #0000cc solid  1px;
  }
  .iconBox{
    width:200%;
    height: 155px;
    padding-top: 1%;
    border: #0000cc solid  1px;
  }
  .flowBox{
    border: #0000cc solid  1px;
    height: 160px;
    white-space: nowrap;
    overflow-y: hidden;
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
