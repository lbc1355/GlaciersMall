<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="名称" prop="name">
      <el-input v-model="dataForm.name" placeholder="名称"></el-input>
    </el-form-item>
    <el-form-item label="商品ID" prop="goodId">
      <el-input v-model="dataForm.goodId" placeholder="商品ID"></el-input>
    </el-form-item>
    <el-form-item label="价格" prop="price">
      <el-input v-model="dataForm.price" placeholder="价格"></el-input>
    </el-form-item>
    <el-form-item label="上次价格" prop="oldPrice">
      <el-input v-model="dataForm.oldPrice" placeholder="上次价格"></el-input>
    </el-form-item>
    <el-form-item label="存库" prop="inventory">
      <el-input v-model="dataForm.inventory" placeholder="存库"></el-input>
    </el-form-item>
    <el-form-item label="有效" prop="isEffective">
      <el-input v-model="dataForm.isEffective" placeholder="有效"></el-input>
    </el-form-item>
    <el-form-item label="扣折" prop="discount">
      <el-input v-model="dataForm.discount" placeholder="扣折"></el-input>
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
          goodId: '',
          price: '',
          oldPrice: '',
          inventory: '',
          isEffective: '',
          discount: ''
        },
        dataRule: {
          name: [
            { required: true, message: '名称不能为空', trigger: 'blur' }
          ],
          goodId: [
            { required: true, message: '商品ID不能为空', trigger: 'blur' }
          ],
          price: [
            { required: true, message: '价格不能为空', trigger: 'blur' }
          ],
          oldPrice: [
            { required: true, message: '上次价格不能为空', trigger: 'blur' }
          ],
          inventory: [
            { required: true, message: '存库不能为空', trigger: 'blur' }
          ],
          isEffective: [
            { required: true, message: '有效不能为空', trigger: 'blur' }
          ],
          discount: [
            { required: true, message: '扣折不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/generator/goodssku/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.goodsSku.name
                this.dataForm.goodId = data.goodsSku.goodId
                this.dataForm.price = data.goodsSku.price
                this.dataForm.oldPrice = data.goodsSku.oldPrice
                this.dataForm.inventory = data.goodsSku.inventory
                this.dataForm.isEffective = data.goodsSku.isEffective
                this.dataForm.discount = data.goodsSku.discount
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
              url: this.$http.adornUrl(`/generator/goodssku/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'goodId': this.dataForm.goodId,
                'price': this.dataForm.price,
                'oldPrice': this.dataForm.oldPrice,
                'inventory': this.dataForm.inventory,
                'isEffective': this.dataForm.isEffective,
                'discount': this.dataForm.discount
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
