<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.qid" placeholder="试题ID" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.question" placeholder="题目" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.type" placeholder="类型" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-input v-model="dataForm.pid" placeholder="试卷ID" clearable></el-input>
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
          <!--<el-upload-->
            <!--:show-file-list="false"-->
            <!--accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel"-->
            <!--action="#"-->
            <!--:on-success="fileUploadSuccess"-->
            <!--:on-error="fileUploadError" :disabled="fileUploadBtnText=='正在导入'"-->
            <!--:before-upload="beforeFileUpload" style="display: inline">-->
            <!--<el-button  type="success" :loading="fileUploadBtnText=='正在导入'"><i class="fa fa-lg fa-level-up"-->
                                                                               <!--style="margin-right: 5px"></i>{{fileUploadBtnText}}-->
            <!--</el-button>-->
          <!--</el-upload>-->
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
        prop="qid"
        header-align="center"
        align="center"
        label="试题Id">
      </el-table-column>
      <el-table-column
        prop="title"
        header-align="center"
        align="center"
        label="试卷名">
      </el-table-column>
      <el-table-column
        prop="question"
        header-align="center"
        align="center"
        label="题目">
      </el-table-column>
      <el-table-column
        :show-overflow-tooltip="true"
        prop="quesmenu"
        header-align="center"
        align="center"
        label="选项">
      </el-table-column>
      <el-table-column
        prop="ansower"
        header-align="center"
        align="center"
        label="答案">
      </el-table-column>
      <!--<el-table-column-->
        <!--prop="score"-->
        <!--header-align="center"-->
        <!--align="center"-->
        <!--label="分数">-->
      <!--</el-table-column>-->
      <el-table-column
        prop="type"
        header-align="center"
        align="center"
        label="类型">
      </el-table-column>
      <el-table-column
        prop="createTime"
        header-align="center"
        align="center"
        label="创建时间">
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
          qid: '',
          pid: '',
          title: '',
          type: '',
          question: ''
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
          url: this.$http.adornUrl('/question/search'),
          method: 'post',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'rows': this.pageSize,
            'keytime': this.dataForm.keytime + '',
            'qid': this.dataForm.qid,
            'pid': this.dataForm.pid,
            'title': this.dataForm.title,
            'type': this.dataForm.type,
            'question': this.dataForm.question
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
      // fileUploadSuccess (response, file, fileList) {
      //   if (response) {
      //     this.$message({type: response.status, message: response.msg})
      //   }
      //   // this.loadEmps()
      //   this.fileUploadBtnText = '导入数据'
      // },
      // fileUploadError (err, file, fileList) {
      //   this.fileUploadBtnText = '导入数据'
      // },
      // beforeFileUpload (file) {
      //   this.fileUploadBtnText = '正在导入'
      //   let formData = new FormData()
      //   formData.append('file', file)
      //   this.$http.post(this.$http.adornUrl('/generator2/loan_settlement_fee/import'), formData)
      //     .then(({data}) => {
      //     if (data && data.code === 0) {
      //     this.$message.success('导入成功')
      //     this.getDataList()
      //   } else {
      //     this.$message.error(data.msg)
      //   }
      // })
      // }
    }
  }
</script>
