<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="户用ID" prop="memberId">
      <el-input v-model="dataForm.memberId" placeholder="户用ID"></el-input>
    </el-form-item>
    <el-form-item label="货人收" prop="receiver">
      <el-input v-model="dataForm.receiver" placeholder="货人收"></el-input>
    </el-form-item>
    <el-form-item label="联系电话" prop="contact">
      <el-input v-model="dataForm.contact" placeholder="联系电话"></el-input>
    </el-form-item>
    <el-form-item label="地区" prop="fullLocation">
      <el-input v-model="dataForm.fullLocation" placeholder="地区"></el-input>
    </el-form-item>
    <el-form-item label="详细地址" prop="address">
      <el-input v-model="dataForm.address" placeholder="详细地址"></el-input>
    </el-form-item>
    <el-form-item label="政编码邮" prop="postalCode">
      <el-input v-model="dataForm.postalCode" placeholder="政编码邮"></el-input>
    </el-form-item>
    <el-form-item label="地址标签" prop="addressTags">
      <el-input v-model="dataForm.addressTags" placeholder="地址标签"></el-input>
    </el-form-item>
    <el-form-item label="否是为默认地址" prop="isDefault">
      <el-input v-model="dataForm.isDefault" placeholder="否是为默认地址"></el-input>
    </el-form-item>
    <el-form-item label="省份代码" prop="provinceCode">
      <el-input v-model="dataForm.provinceCode" placeholder="省份代码"></el-input>
    </el-form-item>
    <el-form-item label="城市代码" prop="cityCode">
      <el-input v-model="dataForm.cityCode" placeholder="城市代码"></el-input>
    </el-form-item>
    <el-form-item label="县城代码" prop="countyCode">
      <el-input v-model="dataForm.countyCode" placeholder="县城代码"></el-input>
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
          memberId: '',
          receiver: '',
          contact: '',
          fullLocation: '',
          address: '',
          postalCode: '',
          addressTags: '',
          isDefault: '',
          provinceCode: '',
          cityCode: '',
          countyCode: ''
        },
        dataRule: {
          memberId: [
            { required: true, message: '户用ID不能为空', trigger: 'blur' }
          ],
          receiver: [
            { required: true, message: '货人收不能为空', trigger: 'blur' }
          ],
          contact: [
            { required: true, message: '联系电话不能为空', trigger: 'blur' }
          ],
          fullLocation: [
            { required: true, message: '地区不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '详细地址不能为空', trigger: 'blur' }
          ],
          postalCode: [
            { required: true, message: '政编码邮不能为空', trigger: 'blur' }
          ],
          addressTags: [
            { required: true, message: '地址标签不能为空', trigger: 'blur' }
          ],
          isDefault: [
            { required: true, message: '否是为默认地址不能为空', trigger: 'blur' }
          ],
          provinceCode: [
            { required: true, message: '省份代码不能为空', trigger: 'blur' }
          ],
          cityCode: [
            { required: true, message: '城市代码不能为空', trigger: 'blur' }
          ],
          countyCode: [
            { required: true, message: '县城代码不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/generator/useraddresses/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.memberId = data.userAddresses.memberId
                this.dataForm.receiver = data.userAddresses.receiver
                this.dataForm.contact = data.userAddresses.contact
                this.dataForm.fullLocation = data.userAddresses.fullLocation
                this.dataForm.address = data.userAddresses.address
                this.dataForm.postalCode = data.userAddresses.postalCode
                this.dataForm.addressTags = data.userAddresses.addressTags
                this.dataForm.isDefault = data.userAddresses.isDefault
                this.dataForm.provinceCode = data.userAddresses.provinceCode
                this.dataForm.cityCode = data.userAddresses.cityCode
                this.dataForm.countyCode = data.userAddresses.countyCode
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
              url: this.$http.adornUrl(`/generator/useraddresses/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'memberId': this.dataForm.memberId,
                'receiver': this.dataForm.receiver,
                'contact': this.dataForm.contact,
                'fullLocation': this.dataForm.fullLocation,
                'address': this.dataForm.address,
                'postalCode': this.dataForm.postalCode,
                'addressTags': this.dataForm.addressTags,
                'isDefault': this.dataForm.isDefault,
                'provinceCode': this.dataForm.provinceCode,
                'cityCode': this.dataForm.cityCode,
                'countyCode': this.dataForm.countyCode
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
