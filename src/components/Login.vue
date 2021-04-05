<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar-box">
        <img src="../assets/logo.png" alt=""/>
      </div>
      <el-form  ref="loginFormRef" label-width="0px" class="login_form" :model="loginForm" :rules="loginFormRules">
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
        </el-form-item>
        <el-form-item class="btn">
          <el-button type="primary" @click="login">登陆</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'login',
  data () {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 20, message: '长度应为3～20字', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 20, message: '长度应为6～20位', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return this.$message.error('登陆失败')
        const { data: result } = await this.$http.post('login', this.loginForm)
        console.log(result)
        if (result.meta.status !== 200) return this.$message.error('用户名或密码错误')
        this.$message.success('登陆成功！')
        window.sessionStorage.setItem('token', result.data.token)
        await this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .login_container{
    background-color: #f7f9fa;
    height: 100%;
  }
  .login_box{
    width: 500px;
    height: 350px;
    background-color: #fff;
    border-radius: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    .avatar-box{
      height: 120px;
      width: 120px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 5px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      img{
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }
  .btn {
    display: flex;
    justify-content: flex-end;
  }
  .login_form{
    position: absolute;
    bottom: 30px;
    width: 100%;
    padding: 0 50px;
    box-sizing: border-box;
  }
</style>
