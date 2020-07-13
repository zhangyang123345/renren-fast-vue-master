<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.projectId" placeholder="案件ID" clearable></el-input>
      </el-form-item>
      <!--<el-form-item>-->
        <!--<el-input v-model="dataForm.name" placeholder="项目名称" clearable></el-input>-->
      <!--</el-form-item>-->
      <!--<el-form-item>-->
        <!--<el-input v-model="dataForm.director" placeholder="类型" clearable></el-input>-->
      <!--</el-form-item>-->
      <el-form-item>
        <el-select  v-model="dataForm.projectType" placeholder="类型" clearable filterable multiple collapse-tags="false">
          <el-option v-for="(item,index) in projectTypes" :key="index" :label="item.value" :value="item.value" >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-select  v-model="dataForm.processImprove" placeholder="厂别" clearable filterable multiple collapse-tags="false">
          <el-option v-for="(item,index) in processImproves" :key="index" :label="item.processImprove" :value="item.processImprove" >
          </el-option>
        </el-select>
      </el-form-item>
      <!--<el-form-item>-->
        <!--<el-select  v-model="dataForm.position" placeholder="职称" clearable>-->
          <!--<el-option v-for="(item,index) in positions" :key="index" :label="item.position" :value="item.position" >-->
          <!--</el-option>-->
        <!--</el-select>-->
      <!--</el-form-item>-->
      <!--<el-form-item>-->
        <!--<el-select  v-model="dataForm.custimerField" placeholder="阶段" clearable>-->
          <!--<el-option v-for="(item,index) in custimerFields" :key="index" :label="item.position" :value="item.position" >-->
          <!--</el-option>-->
        <!--</el-select>-->
      <!--</el-form-item>-->
      <el-form-item>
        <el-select  v-model="dataForm.state" placeholder="状态" clearable filterable multiple collapse-tags="false">
          <el-option v-for="(item,index) in states" :key="index" :label="item.value" :value="item.value" >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-date-picker
          v-model="dataForm.keyTime"
          unlink-panels
          type="daterange"
          value-format="yyyy-MM-dd"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期">
        </el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList('1')">查询</el-button>
        <!--<el-button  type="primary" @click="addOrUpdateHandle()">新增</el-button>-->
        <!--<el-button  type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>-->
          <el-upload
            :show-file-list="false"
            accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel"
            action="#"
            :on-success="fileUploadSuccess"
            :on-error="fileUploadError" :disabled="fileUploadBtnText=='正在导入'"
            :before-upload="beforeFileUpload" style="display: inline">
            <el-button  type="success" :loading="fileUploadBtnText=='正在导入'"><i class="fa fa-lg fa-level-up"
                                                                               style="margin-right: 5px"></i>{{fileUploadBtnText}}
            </el-button>
          </el-upload>
        <el-button type="primary" @click="export2Exce()" ><i class="fa fa-lg fa-level-down"
                                                             style="margin-right: 5px"></i>导出数据
        </el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        prop="projectId"
        header-align="center"
        align="center"
        label="案件Id">
      </el-table-column>
      <el-table-column
        prop="submitEmail"
        header-align="center"
        align="center"
        label="提交人邮箱">
      </el-table-column>
      <el-table-column
        prop="state"
        header-align="center"
        align="center"
        label="状态">
      </el-table-column>
      <el-table-column
        prop="lscDate"
        header-align="center"
        align="center"
        label="最后修改时间">
      </el-table-column>
      <el-table-column
        prop="projectName"
        header-align="center"
        align="center"
        label="项目名称">
      </el-table-column>
      <el-table-column
        prop="projectType"
        header-align="center"
        align="center"
        label="类型">
      </el-table-column>
      <el-table-column
        prop="processImprove"
        header-align="center"
        align="center"
        label="厂别">
      </el-table-column>
      <el-table-column
        prop="processEmail"
        header-align="center"
        align="center"
        label="审核主管邮箱">
      </el-table-column>
      <el-table-column
        prop="teamLeader"
        header-align="center"
        align="center"
        label="项目领导者">
      </el-table-column>
      <el-table-column
        prop="leaderEmail"
        header-align="center"
        align="center"
        label="领导者邮箱">
      </el-table-column>
      <el-table-column
        prop="teamMember"
        header-align="center"
        align="center"
        label="项目队员">
      </el-table-column>
      <el-table-column
        prop="memberEmail"
        header-align="center"
        align="center"
        label="队员邮箱">
      </el-table-column>
      <el-table-column
        prop="customerField"
        header-align="center"
        align="center"
        label="产品阶段">
      </el-table-column>
      <el-table-column
        prop="hardSaving"
        header-align="center"
        align="center"
        label="节省金额">
      </el-table-column>
      <el-table-column
        prop="impacOn"
        header-align="center"
        align="center"
        label="节省类型">
      </el-table-column>
      <el-table-column
        prop="submitDate"
        header-align="center"
        align="center"
        label="提交时间">
      </el-table-column>
      <el-table-column
        prop="closeDate"
        header-align="center"
        align="center"
        label="完成时间">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="80"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.projectId)">详情</el-button>
        </template>
      </el-table-column>
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
  import AddOrUpdate from './case-add-or-update'
  import moment from "moment"
  export default {
    data () {
      return {
        dataForm: {
          keyTime: '',
          projectId: '',
          projectType: '',
          state: ['Closed'],
          custimerField: '',
          processImprove: ['OP2-PVD'],
          impactOn: ''
        },
        loading: '',
        exportList: [],
        impactOns: [],
        processImproves: '',
        projectIds: [],
        projectTypes: [{value: 'Blitz'}, {value: 'A3'}, {value: 'DMAIC'}, {value: 'DMADV'}, {value: '8D'}],
        states: [{value: 'Regional Admin Review'}, {value: 'Closed'}, {value: 'Finance Review'}, {value: 'In Progress'}, {value: 'Planned'}, {value: 'Submitted'}],
        custimerFields: [],
        fileUploadBtnText: '导入案件数据',
        dataList: [],
        positions: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false
      }
    },
    components: {
      AddOrUpdate
    },
    activated () {
      this.getDataList()
    },
    created () {
      // this.getDataJB()
    },
    methods: {
      // 获取基本参数
      getDataJB () {
        // 获取厂别
        this.$http({
          url: this.$http.adornUrl('/project/getProcess'),
          method: 'get',
          data: this.$http.adornData(this.dataList, right)
        }).then(({data}) => {
          if (data && data.code === 0) {
          this.processImproves = data.list
        }
      })
      },
      // 获取数据列表
      getDataList (type) {
        if (type == '1') {
          this.pageIndex = 1
          this.pageSize = 10
          this.totalPage = 0
        }
        if (this.dataForm.keyTime == '' || this.dataForm.keyTime == null || this.dataForm.keyTime == undefined) {
        if (moment(moment().valueOf()).format('MM-DD') < moment('09-01').format('MM-DD')) {
          this.dataForm.keyTime = [moment(moment().add(-1, 'years').valueOf()).format('YYYY-09-01'), moment(moment().valueOf()).format('YYYY-MM-DD')]
        } else {
          this.dataForm.keyTime = [moment(moment().valueOf()).format('YYYY-09-01'), moment(moment().valueOf()).format('YYYY-MM-DD')]
        }
        }
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/project/project'),
          method: 'post',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'rows': this.pageSize,
            'projectId': this.dataForm.projectId,
            'projectType': this.dataForm.projectType + '',
            'state': this.dataForm.state + '',
            'custimerField': this.dataForm.custimerField + '',
            'processImprove': this.dataForm.processImprove + '',
            'keytime': this.dataForm.keyTime + ''
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.data.list
            this.totalPage = data.data.total
          } else {
          this.$message.error(data.msg)
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 导出数据
      export2Exce () {
        this.startLoading()
        this.$http({
          url: this.$http.adornUrl('/project/exportCaseMsg'),
          method: 'post',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'rows': this.pageSize,
            'projectId': this.dataForm.projectId,
            'projectType': this.dataForm.projectType + '',
            'state': this.dataForm.state + '',
            'custimerField': this.dataForm.custimerField + '',
            'processImprove': this.dataForm.processImprove + '',
            'keytime': this.dataForm.keyTime + ''
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.exportList = data.data.list
          require.ensure([], () => {
            const { export_json_to_excel } = require('@/vendor/Export2Excel')
            const tHeader = ['案件ID', '提交人邮箱', '状态', '最后修改时间', '项目名称', '类型', '厂别', '审核主管邮箱', '项目领导者', '领导者邮箱', '项目队员', '队员邮箱', '产品阶段', '节省金额', '节省类型', '提交时间', '完成时间']
            // 上面设置Excel的表格第一行的标题
            const filterVal = ['projectId', 'submitEmail', 'state', 'lscDate', 'projectName', 'projectType', 'processImprove', 'processEmail', 'teamLeader', 'leaderEmail', 'teamMember', 'memberEmail', 'customerField', 'hardSaving', 'impacOn', 'submitDate', 'closeDate']
            const list = this.exportList
            const data = this.formatJson(filterVal, list)
            export_json_to_excel(tHeader, data, '案件信息列表')
        })
        } else {
          this.$message.error(data.msg)
        }
        this.endLoading()
      })
      },
      formatJson (filterVal, jsonData) {
        return jsonData.map(v => filterVal.map(j => v[j]))
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
      // 新增 / 修改
      addOrUpdateHandle (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id)
        })
      },
      fileUploadSuccess (response, file, fileList) {
        if (response) {
          this.$message({type: response.status, message: response.msg})
        }
        // this.loadEmps()
        this.fileUploadBtnText = '导入数据'
      },
      fileUploadError (err, file, fileList) {
      },
      beforeFileUpload (file) {
        this.startLoading()
        this.fileUploadBtnText = '正在导入'
        let formData = new FormData()
        formData.append('file', file)
        this.$http.post(this.$http.adornUrl('/project/import'), formData)
          .then(({data}) => {
          if (data && data.code === 0) {
          this.$message.success('导入成功')
          this.fileUploadBtnText = '导入数据'
          this.getDataList()
        } else {
          this.$message.error(data.msg)
          this.fileUploadBtnText = '导入数据'
        }
        this.endLoading()
      })
      }
    }
  }
</script>
