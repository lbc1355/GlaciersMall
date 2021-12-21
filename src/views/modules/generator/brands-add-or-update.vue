<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="名称" prop="name">
      <el-input v-model="dataForm.name" placeholder="名称"></el-input>
    </el-form-item>
    <el-form-item label="英文名称" prop="nameen">
      <el-input v-model="dataForm.nameen" placeholder="英文名称"></el-input>
    </el-form-item>
    <el-form-item label="商标" prop="logo">
      <el-input v-model="dataForm.logo" placeholder="商标"></el-input>
    </el-form-item>
    <el-form-item label="图片" prop="picture">
      <el-input v-model="dataForm.picture" placeholder="图片"></el-input>
    </el-form-item>
    <el-form-item label="型类" prop="type">
      <el-input v-model="dataForm.type" placeholder="型类"></el-input>
    </el-form-item>
    <el-form-item label="述描" prop="desc">
      <el-input v-model="dataForm.desc" placeholder="述描"></el-input>
    </el-form-item>
    <el-form-item label="地点" prop="place">
      <el-input v-model="dataForm.place" placeholder="地点"></el-input>
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
          name: '',
          nameen: '',
          logo: '',
          picture: '',
          type: '',
          desc: '',
          place: ''
        },
        dataRule: {
          name: [
            { required: true, message: '名称不能为空', trigger: 'blur' }
          ],
          nameen: [
            { required: true, message: '英文名称不能为空', trigger: 'blur' }
          ],
          logo: [
            { required: true, message: '商标不能为空', trigger: 'blur' }
          ],
          picture: [
            { required: true, message: '图片不能为空', trigger: 'blur' }
          ],
          type: [
            { required: true, message: '型类不能为空', trigger: 'blur' }
          ],
          desc: [
            { required: true, message: '述描不能为空', trigger: 'blur' }
          ],
          place: [
            { required: true, message: '地点不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/generator/brands/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.brands.name
                this.dataForm.nameen = data.brands.nameen
                this.dataForm.logo = data.brands.logo
                this.dataForm.picture = data.brands.picture
                this.dataForm.type = data.brands.type
                this.dataForm.desc = data.brands.desc
                this.dataForm.place = data.brands.place
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
              url: this.$http.adornUrl(`/generator/brands/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'nameen': this.dataForm.nameen,
                'logo': this.dataForm.logo,
                'picture': this.dataForm.picture,
                'type': this.dataForm.type,
                'desc': this.dataForm.desc,
                'place': this.dataForm.place
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
