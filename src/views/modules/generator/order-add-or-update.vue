<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="用户ID" prop="memberId">
      <el-input v-model="dataForm.memberId" placeholder="用户ID"></el-input>
    </el-form-item>
    <el-form-item label="付支方式" prop="payChannel">
      <el-input v-model="dataForm.payChannel" placeholder="付支方式"></el-input>
    </el-form-item>
    <el-form-item label="线支付在或者货到付款" prop="payType">
      <el-input v-model="dataForm.payType" placeholder="线支付在或者货到付款"></el-input>
    </el-form-item>
    <el-form-item label="订单状态" prop="orderState">
      <el-input v-model="dataForm.orderState" placeholder="订单状态"></el-input>
    </el-form-item>
    <el-form-item label="费邮" prop="postFee">
      <el-input v-model="dataForm.postFee" placeholder="费邮"></el-input>
    </el-form-item>
    <el-form-item label="实付金额" prop="payMoney">
      <el-input v-model="dataForm.payMoney" placeholder="实付金额"></el-input>
    </el-form-item>
    <el-form-item label="金额合计" prop="totalMoney">
      <el-input v-model="dataForm.totalMoney" placeholder="金额合计"></el-input>
    </el-form-item>
    <el-form-item label="数量合计" prop="totalNum">
      <el-input v-model="dataForm.totalNum" placeholder="数量合计"></el-input>
    </el-form-item>
    <el-form-item label="建创订单时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="建创订单时间"></el-input>
    </el-form-item>
    <el-form-item label="款付时间型类" prop="payTime">
      <el-input v-model="dataForm.payTime" placeholder="款付时间型类"></el-input>
    </el-form-item>
    <el-form-item label="配送时间" prop="deliveryTimeType">
      <el-input v-model="dataForm.deliveryTimeType" placeholder="配送时间"></el-input>
    </el-form-item>
    <el-form-item label="买卖留言" prop="buyerMessage">
      <el-input v-model="dataForm.buyerMessage" placeholder="买卖留言"></el-input>
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
          payChannel: '',
          payType: '',
          orderState: '',
          postFee: '',
          payMoney: '',
          totalMoney: '',
          totalNum: '',
          createTime: '',
          payTime: '',
          deliveryTimeType: '',
          buyerMessage: ''
        },
        dataRule: {
          memberId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          payChannel: [
            { required: true, message: '付支方式不能为空', trigger: 'blur' }
          ],
          payType: [
            { required: true, message: '线支付在或者货到付款不能为空', trigger: 'blur' }
          ],
          orderState: [
            { required: true, message: '订单状态不能为空', trigger: 'blur' }
          ],
          postFee: [
            { required: true, message: '费邮不能为空', trigger: 'blur' }
          ],
          payMoney: [
            { required: true, message: '实付金额不能为空', trigger: 'blur' }
          ],
          totalMoney: [
            { required: true, message: '金额合计不能为空', trigger: 'blur' }
          ],
          totalNum: [
            { required: true, message: '数量合计不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '建创订单时间不能为空', trigger: 'blur' }
          ],
          payTime: [
            { required: true, message: '款付时间型类不能为空', trigger: 'blur' }
          ],
          deliveryTimeType: [
            { required: true, message: '配送时间不能为空', trigger: 'blur' }
          ],
          buyerMessage: [
            { required: true, message: '买卖留言不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/generator/order/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.memberId = data.order.memberId
                this.dataForm.payChannel = data.order.payChannel
                this.dataForm.payType = data.order.payType
                this.dataForm.orderState = data.order.orderState
                this.dataForm.postFee = data.order.postFee
                this.dataForm.payMoney = data.order.payMoney
                this.dataForm.totalMoney = data.order.totalMoney
                this.dataForm.totalNum = data.order.totalNum
                this.dataForm.createTime = data.order.createTime
                this.dataForm.payTime = data.order.payTime
                this.dataForm.deliveryTimeType = data.order.deliveryTimeType
                this.dataForm.buyerMessage = data.order.buyerMessage
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
              url: this.$http.adornUrl(`/generator/order/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'memberId': this.dataForm.memberId,
                'payChannel': this.dataForm.payChannel,
                'payType': this.dataForm.payType,
                'orderState': this.dataForm.orderState,
                'postFee': this.dataForm.postFee,
                'payMoney': this.dataForm.payMoney,
                'totalMoney': this.dataForm.totalMoney,
                'totalNum': this.dataForm.totalNum,
                'createTime': this.dataForm.createTime,
                'payTime': this.dataForm.payTime,
                'deliveryTimeType': this.dataForm.deliveryTimeType,
                'buyerMessage': this.dataForm.buyerMessage
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
