<template>
  <el-form ref="form" :model="userForm" label-width="100px">
    <el-form-item label="头像" prop="avatar">
      <el-upload
          class="avatar-uploader"
          :action="`${defaultConfig.baseApiUrl}/uploadImg`"
          name="blogIllustrations"
          :show-file-list="false"
          :on-success="handleAvatarSuccess"
      >
        <img v-if="userForm.avatar" :src="userForm.avatar" class="avatar">
        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
      </el-upload>
    </el-form-item>
    <el-form-item label="用户信息简介" prop="userName">
      <el-input v-model="userForm.introduction" placeholder="请输入简介信息"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm()">提交</el-button>
      <el-button type="danger" @click="resetForm()">重置</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import AuthorService from "@/service/AuthorService";
import defaultConfig from '@/config/config.default'

export default {
  name: "UserSetting",
  data() {
    return {
      userForm: {
        avatar: '',
        introduction: ''
      },
      defaultConfig
    }
  },
  methods: {
    submitForm() {
      if(this.userForm.avatar&&this.userForm.introduction){
        AuthorService.updateUserInfo(this.userForm).then(() => {
          this.$message.success('用户信息更新成功');
          this.$router.go(0)
        })
      }else {
        this.$message.success('请先完善信息')
      }
    },
    resetForm(){
      this.$refs.form.resetFields()
    },
    handleAvatarSuccess(rs) {
      console.log('头像上传成功', rs.data.imgList[0]);
      this.userForm.avatar = rs.data.imgList[0];
    }
  }
}
</script>

<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.avatar-uploader .el-upload:hover {
  border-color: #409EFF;
}

.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px !important;
  text-align: center;
}

.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>
