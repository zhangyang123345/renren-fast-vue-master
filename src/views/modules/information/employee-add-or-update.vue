<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="工号" prop="jobNo">
      <el-input v-model="dataForm.jobNo" placeholder="工号" ></el-input>
    </el-form-item>
    <el-form-item label="姓名" prop="name">
      <el-input v-model="dataForm.name" placeholder="姓名"></el-input>
    </el-form-item>
    <el-form-item label="备注" prop="comment">
      <el-input v-model="dataForm.comment" placeholder="备注"></el-input>
    </el-form-item>
    <el-form-item label="性别" prop="sex">
      <el-input v-model="dataForm.sex" placeholder="性别"></el-input>
    </el-form-item>
      <el-form-item label="人员类别" prop="pCategory">
        <el-input v-model="dataForm.pCategory" placeholder="人员类别"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="phone">
        <el-input v-model="dataForm.phone" placeholder="联系方式"></el-input>
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="dataForm.email" placeholder="邮箱"></el-input>
      </el-form-item>
      <el-form-item label="厂别" prop="plantType">
        <el-input v-model="dataForm.plantType" placeholder="厂别"></el-input>
      </el-form-item>
      <el-form-item label="部门" prop="department">
        <el-input v-model="dataForm.department" placeholder="部门"></el-input>
      </el-form-item>
      <el-form-item label="职称" prop="position">
        <el-input v-model="dataForm.position" placeholder="职称"></el-input>
      </el-form-item>
      <el-form-item label="成本别" prop="costCategory">
        <el-input v-model="dataForm.costCategory" placeholder="成本别"></el-input>
      </el-form-item>
      <el-form-item label="班别" prop="wclass">
        <el-input v-model="dataForm.wclass" placeholder="班别"></el-input>
      </el-form-item>
      <el-form-item label="线别" prop="lineType">
        <el-input v-model="dataForm.lineType" placeholder="线别"></el-input>
      </el-form-item>
      <el-form-item label="办公楼栋" prop="officeLocation">
        <el-input v-model="dataForm.officeLocation" placeholder="办公楼栋"></el-input>
      </el-form-item>
      <el-form-item label="上级" prop="pjName">
        <el-input v-model="dataForm.pjName" placeholder="厂别"></el-input>
      </el-form-item>
      <el-form-item label="主管" prop="director">
        <el-input v-model="dataForm.director" placeholder="主管"></el-input>
      </el-form-item>
      <el-form-item label="入职日期" prop="entryDate">
        <el-date-picker
          v-model="dataForm.entryDate"
          unlink-panels
          type="date"
          value-format="yyyy-MM-dd"
          placeholder="入职日期">
        </el-date-picker>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
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
          sex: '',
          pCategory: '',
          phone: '',
          plantType: '',
          department: '',
          director: '',
          position: '',
          costCategory: '',
          wclass: '',
          lineType: '',
          officeLocation: '',
          pjName: '',
          entryDate: '',
          email: ''
        },
        dataRule: {
          jobNo: [
            { required: true, message: '工号不能为空', trigger: 'blur' }
          ],
          name: [
            { required: true, message: '姓名不能为空', trigger: 'blur' }
          ],
          // comment: [
          //   { required: true, message: '备注不能为空', trigger: 'blur' }
          // ],
          sex: [
            { required: true, message: '性别不能为空', trigger: 'blur' }
          ],
          pCategory: [
            { required: true, message: '人员类别不能为空', trigger: 'blur' }
          ],
          phone: [
            { required: true, message: '联系方式不能为空', trigger: 'blur' }
          ],
          plantType: [
            { required: true, message: '厂别不能为空', trigger: 'blur' }
          ],
          department: [
            { required: true, message: '部门不能为空', trigger: 'blur' }
          ],
          director: [
            { required: true, message: '上级不能为空', trigger: 'blur' }
          ],
          position: [
            { required: true, message: '职称不能为空', trigger: 'blur' }
          ],
          costCategory: [
            { required: true, message: '成本别不能为空', trigger: 'blur' }
          ],
          wclass: [
            { required: true, message: '班别不能为空', trigger: 'blur' }
          ],
          lineType: [
            { required: true, message: '线别不能为空', trigger: 'blur' }
          ],
          officeLocation: [
            { required: true, message: '办公楼栋不能为空', trigger: 'blur' }
          ],
          pjName: [
            { required: true, message: '主管不能为空', trigger: 'blur' }
          ],
          entryDate: [
        { required: true, message: '入职日期不能为空', trigger: 'blur' }
      ]
        }
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
          this.$refs['dataForm'].resetFields()
          if (id) {
            this.$http({
              url: this.$http.adornUrl('/employee/getEmp'),
              method: 'post',
              params: this.$http.adornParams({
                'jobNo': id
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.jobNo = data.data.jobNo
                this.dataForm.name = data.data.name
              this.dataForm.comment = data.data.comment
              this.dataForm.sex = data.data.sex
              this.dataForm.pCategory = data.data.pCategory
              this.dataForm.phone = data.data.phone
              this.dataForm.plantType = data.data.plantType
              this.dataForm.department = data.data.department
              this.dataForm.position = data.data.position
              this.dataForm.costCategory = data.data.costCategory
              this.dataForm.wclass = data.data.wclass
              this.dataForm.lineType = data.data.lineType
              this.dataForm.officeLocation = data.data.officeLocation
              this.dataForm.pjName = data.data.pjName
                this.dataForm.id = data.data.id
                this.dataForm.entryDate = data.data.entryDate
                this.dataForm.email = data.data.email
                this.dataForm.director = data.data.director
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/employee/${!this.dataForm.id ? 'insert' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'job_no': this.dataForm.jobNo,
                'name': this.dataForm.name,
                'comment': this.dataForm.comment,
                'sex': this.dataForm.sex,
                'p_category': this.dataForm.pCategory,
                'phone': this.dataForm.phone,
                'plant_type': this.dataForm.plantType,
                'cost_category': this.dataForm.costCategory,
                'wclass': this.dataForm.wclass,
                'line_type': this.dataForm.lineType,
                'office_location': this.dataForm.officeLocation,
                'pj_name': this.dataForm.pjName,
                'entry_date': this.dataForm.entryDate,
                'department': this.dataForm.department,
                'director': this.dataForm.director,
                'position': this.dataForm.position,
                'email': this.dataForm.email
                // 'pj_no':this.dataForm.pjNo,
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
