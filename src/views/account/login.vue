<template>
  <el-form
    :model="userForm"
    :rules="rules"
    ref="userForm"
    label-position="left"
    label-width="0px"
    class="demo-userForm login-container"
  >
    <h3 class="title">电商后台管理系统登录</h3>
    <el-form-item prop="username">
      <el-input
        type="text"
        v-model="userForm.username"
        auto-complete="off"
        prefix-icon="iconfont icon-login_user"
        placeholder="账号"
      ></el-input>
    </el-form-item>
    <el-form-item prop="password">
      <el-input
        type="password"
        v-model="userForm.password"
        auto-complete="off"
        prefix-icon="iconfont icon-login_password"
        placeholder="密码"
      ></el-input>
    </el-form-item>
    <el-checkbox
      v-model="checked"
      checked
      class="remember"
    >记住密码</el-checkbox>
    <el-form-item style="width:100%;">
      <el-button
        type="primary"
        style="width:100%;"
        @click.native.prevent="login"
        :loading="logining"
      >登录</el-button>
      <!-- <el-button @click.native.prevent="handleReset2">重置</el-button> -->
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  data() {
    return {
      logining: false,
      checked: true,
      userForm: {
        username: 'admin',
        password: '123456'
      },
      rules: {
        username: [
          { required: true, message: '请输入账号', trigger: 'blur' }
          // { validator: validaePass }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
          // { validator: validaePass2 }
        ]
      }
    }
  },
  methods: {
    async login() {
      this.logining = true
      const result = await this.$axios.post('login', this.userForm)

      this.logining = false
      if (result.data.meta.status === 200) {
        localStorage.setItem('mytoken', result.data.data.token)
        this.$router.push('/layout')
      } else {
        this.$message.error(result.data.meta.msg)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.login-container {
  /*box-shadow: 0 0px 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0px 0 rgba(0, 0, 0, 0.02);*/
  -webkit-border-radius: 5px;
  border-radius: 5px;
  -moz-border-radius: 5px;
  background-clip: padding-box;
  margin: 100px auto;
  width: 350px;
  padding: 35px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
  .title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }
  .remember {
    margin: 0px 0px 35px 0px;
  }
}
</style>
