<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar-box">
        <img src="../assets/logo.png" alt=""/>
      </div>
      <el-form label-width="0px" class="login_form">
        <el-form-item>
          <el-input prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item class="btn">
          <el-button type="primary">登陆</el-button>
          <el-button type="info">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'login'
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
