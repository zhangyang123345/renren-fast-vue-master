<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.jobNo" placeholder="工号" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.name" placeholder="姓名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.director" placeholder="主管" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.pid" placeholder="试卷ID" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.ptype" placeholder="试卷类型" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-select  v-model="dataForm.lineType" placeholder="线别" clearable>
          <el-option v-for="(item,index) in lineTypes" :key="index" :label="item.lineType" :value="item.lineType" >
          </el-option>
        </el-select>
      </el-form-item>
      <!--<el-form-item>-->
        <!--<el-select  v-model="dataForm.supervisor" placeholder="主管" clearable>-->
          <!--<el-option v-for="(item,index) in supervisors" :key="index" :label="item.value" :value="item.id" >-->
          <!--</el-option>-->
        <!--</el-select>-->
      <!--</el-form-item>-->
      <!--<el-form-item>-->
        <!--<el-date-picker-->
          <!--v-model="dataForm.keytime"-->
          <!--unlink-panels-->
          <!--type="daterange"-->
          <!--value-format="yyyy-MM-dd"-->
          <!--range-separator="至"-->
          <!--start-placeholder="开始日期"-->
          <!--end-placeholder="结束日期">-->
        <!--</el-date-picker>-->
      <!--</el-form-item>-->
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
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
        prop="title"
        header-align="center"
        align="center"
        label="试卷名">
      </el-table-column>
      <el-table-column
        prop="ptype"
        header-align="center"
        align="center"
        label="试卷类型">
      </el-table-column>
      <el-table-column
        prop="sscore"
        header-align="center"
        align="center"
        label="总分">
      </el-table-column>
      <el-table-column
        prop="pscore"
        header-align="center"
        align="center"
        label="及格分">
      </el-table-column>
      <el-table-column
        prop="uscore"
        header-align="center"
        align="center"
        label="得分">
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
      <!--<el-table-column-->
        <!--fixed="right"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--width="150"-->
        <!--label="操作">-->
        <!--<template slot-scope="scope">-->
          <!--<el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.goodsId)">修改</el-button>-->
          <!--<el-button type="text" size="small" @click="deleteHandle(scope.row.goodsId)">删除</el-button>-->
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
    <!--<add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>-->
  </div>
</template>

<script>
  // import AddOrUpdate from './employee-add-or-update'
  export default {
    data () {
      return {
        dataForm: {
          keytime: '',
          jobNo: '',
          name: '',
          director: '',
          pid: '',
          ptype: '',
          lineType: ''
        },
        supervisors: [],
        levels: [],
        lineTypes: [],
        fileUploadBtnText: '导入数据',
        dataList: [],
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
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/ansower/search'),
          method: 'post',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'rows': this.pageSize,
            'keytime': this.dataForm.keytime + '',
            'jobNo': this.dataForm.jobNo,
            'name': this.dataForm.name,
            'director': this.dataForm.director,
            'pid': this.dataForm.pid,
            'ptype': this.dataForm.ptype,
            'lineType': this.dataForm.lineType
          })
        }).then(({data}) => {
          console.log(data)
          if (data && data.code === 0) {
            this.dataList = data.data.list
            this.totalPage = data.data.total
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
        // 获取线别
        this.$http({
          url: this.$http.adornUrl('/lineType/search'),
          method: 'post',
          data: this.$http.adornData(this.dataList, false)
        }).then(({data}) => {
          if (data && data.code === 0) {
          this.lineTypes = data.data
        }
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
      },
      // 新增 / 修改
      addOrUpdateHandle (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id)
        })
      },
      // 删除
      deleteHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.goodsId
        })
        this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/generator/tbgoods3/delete'),
            method: 'post',
            data: this.$http.adornData(ids, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      }
    }
  }
</script>
