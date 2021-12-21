<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="名称" prop="name">
      <el-input v-model="dataForm.name" placeholder="名称"></el-input>
    </el-form-item>
    <el-form-item label="描述" prop="desc">
      <el-input v-model="dataForm.desc" placeholder="描述"></el-input>
    </el-form-item>
    <el-form-item label="价格" prop="price">
      <el-input v-model="dataForm.price" placeholder="价格"></el-input>
    </el-form-item>
    <el-form-item label="图片" prop="picture">
      <el-input v-model="dataForm.picture" placeholder="图片"></el-input>
    </el-form-item>
    <el-form-item label="折扣" prop="discount">
      <el-input v-model="dataForm.discount" placeholder="折扣"></el-input>
    </el-form-item>
    <el-form-item label="顺序" prop="orderNum">
      <el-input v-model="dataForm.orderNum" placeholder="顺序"></el-input>
    </el-form-item>
    <el-form-item label="类分ID" prop="categoryId">
      <el-input v-model="dataForm.categoryId" placeholder="类分ID"></el-input>
    </el-form-item>
    <el-form-item label="品牌ID" prop="brandId">
      <el-input v-model="dataForm.brandId" placeholder="品牌ID"></el-input>
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
          desc: '',
          price: '',
          picture: '',
          discount: '',
          orderNum: '',
          categoryId: '',
          brandId: ''
        },
        dataRule: {
          name: [
            { required: true, message: '名称不能为空', trigger: 'blur' }
          ],
          desc: [
            { required: true, message: '描述不能为空', trigger: 'blur' }
          ],
          price: [
            { required: true, message: '价格不能为空', trigger: 'blur' }
          ],
          picture: [
            { required: true, message: '图片不能为空', trigger: 'blur' }
          ],
          discount: [
            { required: true, message: '折扣不能为空', trigger: 'blur' }
          ],
          orderNum: [
            { required: true, message: '顺序不能为空', trigger: 'blur' }
          ],
          categoryId: [
            { required: true, message: '类分ID不能为空', trigger: 'blur' }
          ],
          brandId: [
            { required: true, message: '品牌ID不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/generator/goods/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.goods.name
                this.dataForm.desc = data.goods.desc
                this.dataForm.price = data.goods.price
                this.dataForm.picture = data.goods.picture
                this.dataForm.discount = data.goods.discount
                this.dataForm.orderNum = data.goods.orderNum
                this.dataForm.categoryId = data.goods.categoryId
                this.dataForm.brandId = data.goods.brandId
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
              url: this.$http.adornUrl(`/generator/goods/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'desc': this.dataForm.desc,
                'price': this.dataForm.price,
                'picture': this.dataForm.picture,
                'discount': this.dataForm.discount,
                'orderNum': this.dataForm.orderNum,
                'categoryId': this.dataForm.categoryId,
                'brandId': this.dataForm.brandId
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
