<template>
  <el-dialog
    :title="!dataForm.skuId ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="选中" prop="selected">
      <el-input v-model="dataForm.selected" placeholder="选中"></el-input>
    </el-form-item>
    <el-form-item label="价格" prop="price">
      <el-input v-model="dataForm.price" placeholder="价格"></el-input>
    </el-form-item>
    <el-form-item label="数量" prop="count">
      <el-input v-model="dataForm.count" placeholder="数量"></el-input>
    </el-form-item>
    <el-form-item label="规格" prop="attrsText">
      <el-input v-model="dataForm.attrsText" placeholder="规格"></el-input>
    </el-form-item>
    <el-form-item label="订单ID" prop="memberId">
      <el-input v-model="dataForm.memberId" placeholder="订单ID"></el-input>
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
          skuId: 0,
          selected: '',
          price: '',
          count: '',
          attrsText: '',
          memberId: ''
        },
        dataRule: {
          selected: [
            { required: true, message: '选中不能为空', trigger: 'blur' }
          ],
          price: [
            { required: true, message: '价格不能为空', trigger: 'blur' }
          ],
          count: [
            { required: true, message: '数量不能为空', trigger: 'blur' }
          ],
          attrsText: [
            { required: true, message: '规格不能为空', trigger: 'blur' }
          ],
          memberId: [
            { required: true, message: '订单ID不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.skuId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.skuId) {
            this.$http({
              url: this.$http.adornUrl(`/generator/cart/info/${this.dataForm.skuId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.selected = data.cart.selected
                this.dataForm.price = data.cart.price
                this.dataForm.count = data.cart.count
                this.dataForm.attrsText = data.cart.attrsText
                this.dataForm.memberId = data.cart.memberId
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
              url: this.$http.adornUrl(`/generator/cart/${!this.dataForm.skuId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'skuId': this.dataForm.skuId || undefined,
                'selected': this.dataForm.selected,
                'price': this.dataForm.price,
                'count': this.dataForm.count,
                'attrsText': this.dataForm.attrsText,
                'memberId': this.dataForm.memberId
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
