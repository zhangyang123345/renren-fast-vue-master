<template>
  <el-dialog
    :title="'详情(' + dataForm.jobNo + ')'"
    :close-on-click-modal="false"
    :visible.sync="visible">
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
        prop="submitDate"
        header-align="center"
        align="center"
        label="提交时间">
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
        prop="customerField"
        header-align="center"
        align="center"
        label="产品阶段">
      </el-table-column>
      <el-table-column
        prop="teamLeader"
        header-align="center"
        align="center"
        label="项目领导者">
      </el-table-column>
      <el-table-column
        prop="teamMember"
        header-align="center"
        align="center"
        label="项目队员">
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[5, 10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          jobNo: '',
          name: '',
          comment: '',
          type: '',
          time: ''
        },
        pageIndex: 1,
        pageSize: 5,
        totalPage: 0,
        dataList: [],
        dataListLoading: false
      }
    },
    methods: {
      // jobNo: 0,
      // name: '',
      // comment: '',
      // sex: '',
      // pCategory: '',
      // phone: '',
      // plantType: '',
      // deportment: '',
      // position: '',
      // costCategory: '',
      // wclass: '',
      // lineType: '',
      // officeLocation: '',
      // pjName: ''
      init (id, type, time) {
        this.dataForm.jobNo = id
        this.dataForm.type = type
        this.dataForm.time = time
        this.pageIndex = 1
          this.pageSize = 5
          this.totalPage = 0
        this.visible = true
        this.$nextTick(() => {
            this.$http({
              url: this.$http.adornUrl('/project/empPeoList'),
              method: 'post',
              params: this.$http.adornParams({
                'page': this.pageIndex,
                'rows': this.pageSize,
                'state': type + '',
                'keytime': time + '',
                'jobNo': id
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataList = data.data.list
                this.totalPage = data.data.total
            }
            })
        })
      },
      getDataList () {
        this.$nextTick(() => {
          this.$http({
          url: this.$http.adornUrl('/project/empPeoList'),
          method: 'post',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'rows': this.pageSize,
            'state': this.dataForm.type + '',
            'keytime': this.dataForm.time + '',
            'jobNo': this.dataForm.jobNo
          })
        }).then(({data}) => {
          console.log(data)
        if (data && data.code === 0) {
          this.dataList = data.data.list
          this.totalPage = data.data.total
        }
      })
      })
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
      }
    }
  }
</script>
