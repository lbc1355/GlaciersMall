<template>
  <el-dialog
    :title="!dataForm.code ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="城市登记" prop="level">
      <el-input v-model="dataForm.level" placeholder="城市登记"></el-input>
    </el-form-item>
    <el-form-item label="城市名称" prop="name">
      <el-input v-model="dataForm.name" placeholder="城市名称"></el-input>
    </el-form-item>
    <el-form-item label="父级ID" prop="parentId">
      <el-input v-model="dataForm.parentId" placeholder="父级ID"></el-input>
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
          code: 0,
          level: '',
          name: '',
          parentId: ''
        },
        dataRule: {
          level: [
            { required: true, message: '城市登记不能为空', trigger: 'blur' }
          ],
          name: [
            { required: true, message: '城市名称不能为空', trigger: 'blur' }
          ],
          parentId: [
            { required: true, message: '父级ID不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.code = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.code) {
            this.$http({
              url: this.$http.adornUrl(`/generator/city/info/${this.dataForm.code}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.level = data.city.level
                this.dataForm.name = data.city.name
                this.dataForm.parentId = data.city.parentId
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
              url: this.$http.adornUrl(`/generator/city/${!this.dataForm.code ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'code': this.dataForm.code || undefined,
                'level': this.dataForm.level,
                'name': this.dataForm.name,
                'parentId': this.dataForm.parentId
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
