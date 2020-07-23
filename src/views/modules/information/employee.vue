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
        <el-select  v-model="dataForm.lineType" placeholder="线别" clearable filterable>
          <el-option v-for="(item,index) in lineTypes" :key="index" :label="item.lineType" :value="item.lineType" >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-select  v-model="dataForm.position" placeholder="职称" clearable>
          <el-option v-for="(item,index) in positions" :key="index" :label="item.position" :value="item.position" >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-select  v-model="dataForm.active" placeholder="状态" clearable>
          <el-option v-for="(item,index) in actives" :key="index" :label="item.value" :value="item.id" >
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
          <!--v-model="dataForm.keyTime"-->
          <!--unlink-panels-->
          <!--type="daterange"-->
          <!--value-format="yyyy-MM-dd"-->
          <!--range-separator="至"-->
          <!--start-placeholder="开始日期"-->
          <!--end-placeholder="结束日期">-->
        <!--</el-date-picker>-->
      <!--</el-form-item>-->
      <el-form-item>
        <el-button @click="getDataList('1')">查询</el-button>
        <el-button  type="primary" @click="addOrUpdateHandle()">新增</el-button>
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
        <el-upload
          :show-file-list="false"
          accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel"
          action="#"
          :on-success="fileUploadSuccess1"
          :on-error="fileUploadError1" :disabled="fileUploadBtnText1=='正在导入'"
          :before-upload="beforeFileUpload1" style="display: inline">
          <el-button  type="success" :loading="fileUploadBtnText1=='正在导入'"><i class="fa fa-lg fa-level-up"
                                                                             style="margin-right: 5px"></i>{{fileUploadBtnText1}}
          </el-button>
        </el-upload>
        <el-upload
          :show-file-list="false"
          accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,application/vnd.ms-excel"
          action="#"
          :on-success="fileUploadSuccess2"
          :on-error="fileUploadError2" :disabled="fileUploadBtnText2=='正在导入'"
          :before-upload="beforeFileUpload2" style="display: inline">
          <el-button  type="success" :loading="fileUploadBtnText2=='正在导入'"><i class="fa fa-lg fa-level-up"
                                                                              style="margin-right: 5px"></i>{{fileUploadBtnText2}}
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
        prop="comment"
        header-align="center"
        align="center"
        label="备注">
      </el-table-column>
      <el-table-column
        prop="sex"
        header-align="center"
        align="center"
        label="性别">
      </el-table-column>
      <el-table-column
        prop="pCategory"
        header-align="center"
        align="center"
        label="人员类别">
      </el-table-column>
      <el-table-column
        prop="phone"
        header-align="center"
        align="center"
        label="联系方式">
      </el-table-column>
      <el-table-column
        prop="email"
        header-align="center"
        align="center"
        label="邮箱">
      </el-table-column>
      <el-table-column
        prop="plantType"
        header-align="center"
        align="center"
        label="厂别">
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
      <el-table-columnposition
        prop="costCategory"
        header-align="center"
        align="center"
        label="成本别">
      </el-table-columnposition>
      <el-table-column
        prop="wclass"
        header-align="center"
        align="center"
        label="班别">
      </el-table-column>
      <el-table-column
        prop="lineType"
        header-align="center"
        align="center"
        label="线别">
      </el-table-column>
      <el-table-column
        prop="officeLocation"
        header-align="center"
        align="center"
        label="办公楼栋">
      </el-table-column>
      <el-table-column
        prop="pjName"
        header-align="center"
        align="center"
        label="上级">
      </el-table-column>
      <el-table-column
        prop="director"
        header-align="center"
        align="center"
        label="主管">
      </el-table-column>
      <el-table-column
        prop="entryDate"
        header-align="center"
        align="center"
        label="入职日期">
      </el-table-column>
      <el-table-column
        prop="active"
        header-align="center"
        align="center"
        label="状态">
        <template scope="scope">
          <div v-if="scope.row.active==0">离职</div>
          <div v-if="scope.row.active==1">在职</div>
          <div v-if="scope.row.active==3">异常</div>
        </template>
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.jobNo)">修改</el-button>
          <el-button v-if="scope.row.active == 1" type="text" size="small" @click="changeStata(scope.row.jobNo,0)">离职</el-button>
          <el-button v-if="scope.row.active == 0" type="text" size="small" @click="changeStata(scope.row.jobNo,1)">在职</el-button>
          <el-button v-if="scope.row.active == 3" type="text" size="small" @click="changeStata(scope.row.jobNo,1)">在职</el-button>
          <el-button v-if="scope.row.active == 3" type="text" size="small" @click="changeStata(scope.row.jobNo,0)">离职</el-button>
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
          keyTime: '',
          key: '',
          level: '',
          lineType: '',
          supervisor: '',
          jobNo: '',
          active: 1,
          name: '',
          palntType: '',
          position: '',
          director: ''
        },
        actives: [{id: 0, value: '离职'}, {id: 1, value: '在职'},{id: 3, value: '异常'}],
        supervisors: [],
        levels: [],
        lineTypes: [],
        fileUploadBtnText: '导入在职数据',
        fileUploadBtnText1: '导入离职数据',
        fileUploadBtnText2: '导入邮箱数据',
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
    methods: {
      // 获取数据列表
      getDataList (type) {
        if(type == '1'){
          this.pageIndex = 1
            this.pageSize = 10
            this.totalPage = 0
        }
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/employee/search'),
          method: 'post',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'rows': this.pageSize,
            'jobNo': this.dataForm.jobNo,
            'name': this.dataForm.name,
            'lineType': this.dataForm.lineType,
            'position': this.dataForm.position,
            'director': this.dataForm.director,
            'active': this.dataForm.active
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
        // 获取职称
        this.$http({
          url: this.$http.adornUrl('/position/search'),
          method: 'post',
          data: this.$http.adornData(this.dataList, false)
        }).then(({data}) => {
          console.log(data)
          if (data && data.code === 0) {
          this.positions = data.data
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
      // 修改状态
      changeStata (id, type) {
        this.$confirm(`确定对[id=${id}]进行修改操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/employee/setQuit'),
            method: 'post',
          params: this.$http.adornParams({
            'jobNo': id,
            'active': type
          })
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
      beforeFileUpload (file) {
        this.fileUploadBtnText = '正在导入'
        let formData = new FormData()
        formData.append('file', file)
        this.$http.post(this.$http.adornUrl('/employee/import'), formData)
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
        this.$http.post(this.$http.adornUrl('/employee/uploadQuitEmpl'), formData)
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
      },
      fileUploadSuccess2 (response, file, fileList) {
        if (response) {
          this.$message({type: response.status, message: response.msg})
        }
        // this.loadEmps()
        this.fileUploadBtnText2 = '导入邮箱数据'
      },
      fileUploadError2 (err, file, fileList) {
        // this.fileUploadBtnText1 = '正在导入'
      },
      beforeFileUpload2 (file) {
        this.fileUploadBtnText2 = '正在导入'
        let formData = new FormData()
        formData.append('file', file)
        this.$http.post(this.$http.adornUrl('/employee/upEmail'), formData)
          .then(({data}) => {
          if (data && data.code === 0) {
          this.$message.success('导入成功')
          this.fileUploadBtnText2 = '导入邮箱数据'
          this.getDataList()
        } else {
          this.$message.error(data.msg)
          this.fileUploadBtnText2 = '导入邮箱数据'
        }
      })
      }
    }
  }
</script>
