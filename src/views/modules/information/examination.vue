<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.pid" placeholder="试卷ID" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.title" placeholder="试卷名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.ptype" placeholder="试卷类型" clearable></el-input>
      </el-form-item>
      <!--<el-form-item>-->
        <!--<el-select  v-model="dataForm.supervisor" placeholder="主管" clearable>-->
          <!--<el-option v-for="(item,index) in supervisors" :key="index" :label="item.value" :value="item.id" >-->
          <!--</el-option>-->
        <!--</el-select>-->
      <!--</el-form-item>-->
      <el-form-item>
        <el-date-picker
          v-model="dataForm.keytime"
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
        prop="pid"
        header-align="center"
        align="center"
        label="试卷Id">
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
        label="试卷总分">
      </el-table-column>
      <el-table-column
        prop="pscore"
        header-align="center"
        align="center"
        label="合格分数">
      </el-table-column>
      <el-table-column
        prop="stime"
        header-align="center"
        align="center"
        label="限制时间(min)">
      </el-table-column>
      <el-table-column
        prop="createTime"
        header-align="center"
        align="center"
        label="创建时间">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <!--<el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.goodsId)">修改</el-button>-->
          <el-upload
            :show-file-list="false"
            accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel"
            action="#"
            :on-success="fileUploadSuccess1"
            :on-error="fileUploadError1" :disabled="fileUploadBtnText1=='正在导入'"
            :before-upload="beforeFileUpload1" style="display: inline">
            <el-button @click="urladd(scope.row.pid)" type="text" size="small" :loading="fileUploadBtnText1=='正在导入'"><i class="fa fa-lg fa-level-up"
                                                                               style="margin-right: 5px"></i>{{fileUploadBtnText1}}
            </el-button>
          </el-upload>
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
  import AddOrUpdate from './employee-add-or-update'
  export default {
    data () {
      return {
        dataForm: {
          keytime: '',
          pid: '',
          sendpid: '',
          title: '',
          ptype: ''
        },
        supervisors: [],
        levels: [],
        ptypes: [],
        fileUploadBtnText: '导入数据',
        fileUploadBtnText1: '导入考试信息',
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
      AddOrUpdate
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/exampaper/search'),
          method: 'post',
          params: this.$http.adornParams({
        'page': this.pageIndex,
          'rows': this.pageSize,
          'pid': this.dataForm.pid,
          'title': this.dataForm.title,
          'ptype': this.dataForm.ptype,
          'keytime': this.dataForm.keytime + ''
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.data.list
            this.totalPage = data.data.total
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // url地址
      urladd (id) {
       this.dataForm.sendpid = id
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
        // this.fileUploadBtnText = '正在导入'
      },
      beforeFileUpload (file, data) {
        this.fileUploadBtnText = '正在导入'
        let formData = new FormData()
        formData.append('file', file)
        this.$http.post(this.$http.adornUrl('/exampaper/import'), formData)
          .then(({data}) => {
          if (data && data.code === 0) {
          this.$message.success('导入成功')
          this.fileUploadBtnText = '导入数据'
          this.getDataList()
        } else {
          this.$message.error(data.msg)
          this.fileUploadBtnText = '导入数据'
        }
      })
      },
      fileUploadSuccess1 (response, file, fileList) {
        if (response) {
          this.$message({type: response.status, message: response.msg})
        }
        // this.loadEmps()
        this.fileUploadBtnText1 = '导入数据'
      },
      fileUploadError1 (err, file, fileList) {
        // this.fileUploadBtnText1 = '正在导入'
      },
      beforeFileUpload1 (file) {
        this.fileUploadBtnText1 = '正在导入'
        let formData = new FormData()
        formData.append('file', file)
        formData.append('pid', this.dataForm.sendpid)
        this.$http.post(this.$http.adornUrl('/ansower/import'), formData)
          .then(({data}) => {
          if (data && data.code === 0) {
          this.$message.success('导入成功')
          this.fileUploadBtnText1 = '导入数据'
          this.getDataList()
        } else {
          this.$message.error(data.msg)
          this.fileUploadBtnText1 = '导入数据'
        }
      })
      }
    }
  }
</script>
