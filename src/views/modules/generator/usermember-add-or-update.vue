<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="账号" prop="account">
      <el-input v-model="dataForm.account" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="password">
      <el-input v-model="dataForm.password" placeholder="密码"></el-input>
    </el-form-item>
    <el-form-item label="昵称" prop="nickname">
      <el-input v-model="dataForm.nickname" placeholder="昵称"></el-input>
    </el-form-item>
    <el-form-item label="创建者" prop="creator">
      <el-input v-model="dataForm.creator" placeholder="创建者"></el-input>
    </el-form-item>
    <el-form-item label="删除" prop="isDelete">
      <el-input v-model="dataForm.isDelete" placeholder="删除"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createTime">
      <el-input v-model="dataForm.createTime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="新更时间" prop="updateTime">
      <el-input v-model="dataForm.updateTime" placeholder="新更时间"></el-input>
    </el-form-item>
    <el-form-item label="" prop="mobile">
      <el-input v-model="dataForm.mobile" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="unionId">
      <el-input v-model="dataForm.unionId" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="source">
      <el-input v-model="dataForm.source" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="avatar">
      <el-input v-model="dataForm.avatar" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="gender">
      <el-input v-model="dataForm.gender" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="birthday">
      <el-input v-model="dataForm.birthday" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="citycode">
      <el-input v-model="dataForm.citycode" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="profession">
      <el-input v-model="dataForm.profession" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="" prop="provincecode">
      <el-input v-model="dataForm.provincecode" placeholder=""></el-input>
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
          account: '',
          password: '',
          nickname: '',
          creator: '',
          isDelete: '',
          createTime: '',
          updateTime: '',
          mobile: '',
          unionId: '',
          source: '',
          avatar: '',
          gender: '',
          birthday: '',
          citycode: '',
          profession: '',
          provincecode: ''
        },
        dataRule: {
          account: [
            { required: true, message: '账号不能为空', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '密码不能为空', trigger: 'blur' }
          ],
          nickname: [
            { required: true, message: '昵称不能为空', trigger: 'blur' }
          ],
          creator: [
            { required: true, message: '创建者不能为空', trigger: 'blur' }
          ],
          isDelete: [
            { required: true, message: '删除不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          updateTime: [
            { required: true, message: '新更时间不能为空', trigger: 'blur' }
          ],
          mobile: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          unionId: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          source: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          avatar: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          gender: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          birthday: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          citycode: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          profession: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          provincecode: [
            { required: true, message: '不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/generator/usermember/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.account = data.userMember.account
                this.dataForm.password = data.userMember.password
                this.dataForm.nickname = data.userMember.nickname
                this.dataForm.creator = data.userMember.creator
                this.dataForm.isDelete = data.userMember.isDelete
                this.dataForm.createTime = data.userMember.createTime
                this.dataForm.updateTime = data.userMember.updateTime
                this.dataForm.mobile = data.userMember.mobile
                this.dataForm.unionId = data.userMember.unionId
                this.dataForm.source = data.userMember.source
                this.dataForm.avatar = data.userMember.avatar
                this.dataForm.gender = data.userMember.gender
                this.dataForm.birthday = data.userMember.birthday
                this.dataForm.citycode = data.userMember.citycode
                this.dataForm.profession = data.userMember.profession
                this.dataForm.provincecode = data.userMember.provincecode
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
              url: this.$http.adornUrl(`/generator/usermember/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'account': this.dataForm.account,
                'password': this.dataForm.password,
                'nickname': this.dataForm.nickname,
                'creator': this.dataForm.creator,
                'isDelete': this.dataForm.isDelete,
                'createTime': this.dataForm.createTime,
                'updateTime': this.dataForm.updateTime,
                'mobile': this.dataForm.mobile,
                'unionId': this.dataForm.unionId,
                'source': this.dataForm.source,
                'avatar': this.dataForm.avatar,
                'gender': this.dataForm.gender,
                'birthday': this.dataForm.birthday,
                'citycode': this.dataForm.citycode,
                'profession': this.dataForm.profession,
                'provincecode': this.dataForm.provincecode
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
