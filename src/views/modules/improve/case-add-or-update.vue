<template>
  <el-dialog
    :title="'详情(' + dataForm.name + ')'"
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
      <!--<el-table-column-->
        <!--:show-overflow-tooltip="true"-->
        <!--prop="projectName"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="项目名称">-->
      <!--</el-table-column>-->
      <el-table-column
      prop="projectType"
      header-align="center"
      align="center"
      label="项目类型">
    </el-table-column>
      <el-table-column
        prop="state"
        header-align="center"
        align="center"
        label="项目状态">
      </el-table-column>
      <el-table-column
        prop="jobNo"
        header-align="center"
        align="center"
        label="工号">
      </el-table-column>
      <el-table-column
        prop="name"
        header-align="center"
        align="center"
        label="姓名">
      </el-table-column>
      <el-table-column
        prop="department"
        header-align="center"
        align="center"
        label="部门">
      </el-table-column>
      <el-table-column
        prop="position"
        header-align="center"
        align="center"
        label="职称">
      </el-table-column>
      <el-table-column
        prop="lineType"
        header-align="center"
        align="center"
        label="线别">
      </el-table-column>
      <el-table-column
        prop="director"
        header-align="center"
        align="center"
        label="主管">
      </el-table-column>
      <el-table-column
        prop="role"
        header-align="center"
        align="center"
        label="角色">
      </el-table-column>
    </el-table>
    <!--<el-pagination-->
      <!--@size-change="sizeChangeHandle"-->
      <!--@current-change="currentChangeHandle"-->
      <!--:current-page="pageIndex"-->
      <!--:page-sizes="[10, 20, 50, 100]"-->
      <!--:page-size="pageSize"-->
      <!--:total="totalPage"-->
      <!--layout="total, sizes, prev, pager, next, jumper">-->
    <!--</el-pagination>-->
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          projectId: '',
          name: '',
          comment: ''
        },
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
      init (id) {
        this.dataForm.id = id
        this.visible = true
        this.$nextTick(() => {
            this.$http({
              url: this.$http.adornUrl('/project/queryProject'),
              method: 'post',
              params: this.$http.adornParams({
                'projectId': id
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataList = data.list
          this.dataForm.name = data.list[0].projectName

            }
            })
        })
      },
      // 多选
      selectionChangeHandle (val) {
        this.dataListSelections = val
      }
    }
  }
</script>
