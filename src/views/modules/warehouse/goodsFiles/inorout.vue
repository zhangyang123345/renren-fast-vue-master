<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <!--<el-form-item>-->
        <!--<el-input v-model="dataForm.articleId" placeholder="物件号" clearable></el-input>-->
      <!--</el-form-item>-->
      <el-form-item>
        <el-autocomplete
          v-model="dataForm.goodsName"
          :fetch-suggestions="querySearchAsync"
          placeholder="商品名"
          @select="handleSelect"
        ></el-autocomplete>
      </el-form-item>
      <el-form-item>
        <el-select  v-model="dataForm.storeId" placeholder="请选择仓库" clearable>
          <el-option v-for="(item,index) in storeArr" :key="index" :label="item.storename" :value="item.storeId" >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.department" placeholder="需求部门" clearable ></el-input>
      </el-form-item>
      <el-form-item>
        <el-select  v-model="dataForm.msgType" placeholder="请选择类型" clearable>
          <el-option v-for="(item,index) in msgtypes" :key="index" :label="item.value" :value="item.id" >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-date-picker
          v-model="dataForm.keytwo"
          unlink-panels
          type="daterange"
          value-format="yyyy-MM-dd"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期">
        </el-date-picker>
        </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <!--<el-button v-if="isAuth('generator:tbgoods3:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>-->
        <!--<el-button v-if="isAuth('generator:tbgoods3:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>-->
      </el-form-item>
      <!--<el-button slot="reference" type="primary" size="mini" style="margin-left: 5px"-->
                 <!--@click="showAdvanceSearchView"><i-->
        <!--class="fa fa-lg" v-bind:class="[advanceSearchViewVisible ? faangledoubleup:faangledoubledown]"-->
        <!--style="margin-right: 5px"></i>高级搜索-->
      <!--</el-button>-->
    </el-form>
    <el-table
      :data="msg_dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        prop="msgId"
        header-align="center"
        align="center"
        label="信息ID">
      </el-table-column>
      <el-table-column
        prop="goodsName"
        header-align="center"
        align="center"
        label="商品名称">
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="articleId"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="物件号">-->
        <!--<template scope="scope">-->
          <!--<div v-if="scope.row.articleId!=null" v-text="scope.row.articleId"></div>-->
          <!--<div v-else="">暂无数据</div>-->
        <!--</template>-->
      <!--</el-table-column>-->
      <el-table-column
        prop="manufacturer"
        header-align="center"
        align="center"
        label="厂商">
      </el-table-column>
      <el-table-column
        prop="storename"
        header-align="center"
        align="center"
        label="所属仓库">
      </el-table-column>
      <el-table-column
        prop="price"
        header-align="center"
        align="center"
        label="单价">
      </el-table-column>
      <el-table-column
        prop="num"
        header-align="center"
        align="center"
        label="数量">
      </el-table-column>
      <el-table-column
        prop="total"
        header-align="center"
        align="center"
        label="总价">
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="num"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="需求部门">-->
        <!--<template scope="scope">-->
          <!--<div v-if="scope.row.num!=null"></div>-->
          <!--<div v-else="">暂无数据</div>-->
        <!--</template>-->
      <!--</el-table-column>-->
      <el-table-column
        prop="msgType"
        header-align="center"
        align="center"
        label="状态">
        <template scope="scope">
          <div v-if="scope.row.msgType==1">入库</div>
          <!--<div v-if="scope.row.msgType==2">待出库</div>-->
          <div v-if="scope.row.msgType==3">出库</div>
          <div v-if="scope.row.msgType==4">报废</div>
        </template>
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="special"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="专案">-->
        <!--<template scope="scope">-->
          <!--<div v-show="scope.row.special==1">Alaska</div>-->
          <!--<div v-show="scope.row.special==2">Boston</div>-->
          <!--<div v-show="scope.row.special==2">Toronto</div>-->
        <!--</template>-->
      <!--</el-table-column>-->
      <el-table-column
        prop="username1"
        header-align="center"
        align="center"
        label="操作人员">
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="username2"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="出库操作人员">-->
        <!--<template scope="scope">-->
          <!--<div v-if="scope.row.username2!=null" v-text="scope.row.username2"></div>-->
          <!--<div v-else="scope.row.username2==null">暂无数据</div>-->
        <!--</template>-->
      <!--</el-table-column>-->
      <el-table-column
        prop="inTime"
        header-align="center"
        align="center"
        label="时间">
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="outTime"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="出库时间">-->
      <!--</el-table-column>-->
      <!--<el-table-column-->
        <!--fixed="right"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--width="150"-->
        <!--label="操作">-->
        <!--<template slot-scope="scope">-->
          <!--<el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.goodsId)">查看订单信息</el-button>-->
          <!--&lt;!&ndash;<el-button type="text" size="small" @click="deleteHandle(scope.row.goodsId)">删除</el-button>&ndash;&gt;-->
        <!--</template>-->
      <!--</el-table-column>-->
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
  </div>
</template>

<script>
  // import AddOrUpdate from './tbgoods3-add-or-update'
  export default {
    data () {
      return {
        dataForm: {
          key: '',
          keytwo: '',
      goodsName: '',
          articleId: '',
          goodsId: '',
          storeId: '',
          department: '',
          msgType: ''
        },
        // faangledoubleup: 'fa-angle-double-up',
        // faangledoubledown: 'fa-angle-double-down',
        // advanceSearchViewVisible: false,
        dataList: [],
        msg_dataList: [],
        storeArr: [],
        msgtypes: [{id: '1', value: '入库'}, {id: '3', value: '出库'}],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false
      }
    },
    components: {
      // AddOrUpdate
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // //高级搜索按钮
      // showAdvanceSearchView () {
      //   this.advanceSearchViewVisible = !this.advanceSearchViewVisible
      //   this.keywords = ''
      //   if (!this.advanceSearchViewVisible) {
      //     this.emptyEmpData()
      //     this.beginDateScope = ''
      //     // this.loadEmps()
      //   }
      // },
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/goodsFiles/inorout/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'key_time': this.dataForm.keytwo + '',
            'limit': this.pageSize,
            'articleId': this.dataForm.articleId,
            'goodsId': this.dataForm.goodsId,
            'storeId': this.dataForm.storeId,
            'msgType': this.dataForm.msgType,
            'department': this.dataForm.department
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
          console.log(data)
          this.msg_dataList = data.list
          this.totalPage = data.length
        } else {
          this.msg_dataList = []
          this.totalPage = 0
        }
      })
        this.$http({
          url: this.$http.adornUrl('/store/stores/list'),
          method: 'get',
          data: this.$http.adornData(this.dataList, false)
        }).then(({data}) => {
          if (data && data.code === 0) {
          this.storeArr = data.page.list
        }
      })
        this.dataListLoading = false
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },
      // 多选
      selectionChangeHandle (val) {
        this.dataListSelections = val
      },
      querySearchAsync(queryString, cb) {
        // var restaurants = this.restaurants;
        this.$http({

          url: this.$http.adornUrl("/store/goods/goodsInfo"),

          method: "post",

          params: this.$http.adornParams({

            goods_name:queryString

          })

        }).then(({ data }) => {
          this.newrestaurants = data.list

        for(var i=0;i<data.list.length;i++){

          this.newrestaurants[i].value = this.newrestaurants[i].goodsName;

        }
        // console.log("this.newrestaurants="+JSON.stringify(this.newrestaurants))
        // var results = queryString ? data.page.list.filter(this.createStateFilter(queryString)) : data.page.list;

        clearTimeout(this.timeout);
        this.timeout = setTimeout(() => {
          cb(this.newrestaurants);
      }, 100 * Math.random());
      });
      },
      handleSelect(item) {
        this.dataForm.goodsName = item.goodsName
        this.dataForm.goodsId = item.goodsId
      }
      // 新增 / 修改
      // addOrUpdateHandle (id) {
      //   this.addOrUpdateVisible = true
      //   this.$nextTick(() => {
      //     this.$refs.addOrUpdate.init(id)
      //   })
      // },
    }
  }
</script>
