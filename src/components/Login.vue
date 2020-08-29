<template>
    <div class="container">
      <div class="loginBox">
        <h2 @click="rotate()">^_login_^</h2>
        <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules">
          <el-form-item class="item" prop="username">
            <input v-model="loginForm.username" />
            <label>用户名称</label>
          </el-form-item>
          <el-form-item class="item" prop="password">
            <input type="password" v-model="loginForm.password" />
            <label>登录密码</label>
          </el-form-item>
          <el-form-item class="btns">
            <button class="btn" @click="login()">
              登录
              <span></span>
              <span></span>
              <span></span>
              <span></span>
            </button>
            <button class="btn2" @click="resetLoginForm">重置</button>
          </el-form-item>
        </el-form>
        <div class="fluorescence fluorescence-filter" :style="{background:background}"></div>
        <div class="fluorescence fluorescence-border" :style="{background:background}"></div>
        <div class="watch"><p>注：登录按钮的动画样式问题，第一次点击登录时可能需要刷新一次界面，第二次登录就可以进入系统了！</p></div>
      </div>
    </div>
</template>

<script>
export default {
  mounted() {
    document.querySelector('body').setAttribute('style', 
    'background:-webkit-linear-gradient(#1b1b1b,#000)'),
    this.rotate()
  },
  Destroyed() {
    document.querySelector('body').removeAttribute('style')
  },
  data() {
    return {
      //流光的外表层
      angle: 235,
      background: 'linear-gradient(235deg,#89ff00,#060c21,#00bcd4)',
      timer: 0,
      // 星星
      // 星星
      starsCount: 800,
      distance: 800,
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456',
      },

      // 这是表单的验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' },
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' },
        ],
      },
    }
  },
  methods: {
    login() {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功')
        // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
        //   1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
        //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
        window.sessionStorage.setItem('token', res.data.token)
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push('/home')
      })
    },
     // 点击重置按钮，重置登录表单
    resetLoginForm() {
      // console.log(this);
      this.$refs.loginFormRef.resetFields()
    },
    // 背光旋转
    rotate() {
      var that = this
      if (this.timer != 0) {
        clearInterval(this.timer)
        this.timer = 0
      }
      this.timer = setInterval(function () {
        that.angle++
        that.background = 'linear-gradient(' + that.angle + 'deg,#89ff00,#060c21,#00bcd4)'
        if (that.angle == 595) {
          that.angle = 235
        }
      }, 25)
    },
  },
}
</script>

<style scoped>
input,
button {
  background: transparent;
  border: none;
  outline: none;
}

.container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
/* 登录盒子 */
.loginBox {
  position: relative;

  width: 405px;
  height: 460px;
  background: #0b0e14;
  box-shadow: 0px, 15px, 25px, 0 rgba(0, 0, 0, 0.6);
  padding: 40px;
  box-sizing: border-box;
  align-items: center;
}
.fluorescence {
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: linear-gradient(235deg, #89ff00, #060c21, #05bcd4);
}
.fluorescence-filter {
  z-index: -1;
}
.fluorescence-border {
  z-index: -2;
  filter: blur(35px);
}

.loginBox h2 {
  text-align: center;
  color: rgb(211, 211, 211);
  margin:-20px 0 20px 0;
  cursor: pointer;
}
.loginBox h2:hover{
  color:#05bcd4;
}
.item {
  height: 45px;
  border-bottom: 1px solid #fff;
  margin-top: 40px;
  position: relative;
}
.item input {
  width: 100%;
  height: 100%;
  color: white;
  padding-top: 20px;
  font-size: 18px;
  box-sizing: border-box;
}
.item input:focus + label {
  bottom: 23px;
  font-size: 12px;
}
.item label {
  position: absolute;
  left: 0px;
  bottom: 25px;
  color: #03a9f4;
  transition: all 0.5s linear;
}

/* 按钮的设置及动画 */
.btns{
  margin-top: 50px;
}
.btn {
  height: 50px;
  width: 100px;
  margin: 10px;
  color: #03e9f4;
  overflow: hidden;
  position: relative;
  text-transform: uppercase;
  letter-spacing: 2px;
  cursor: pointer;
}
.btn:hover {
  background: #03e9f4;
  border-radius: 5px;
  color: #fff;
  box-shadow: 0 0 5px 0 #03e9f4, 0 0 25px 0 #03e9f4, 0 0 50px 0 #03e9f4, 0 0 100px 0 #03e9f4;
}
.btn > span {
  position: absolute;
}
.btn > span:nth-child(1) {
  width: 100%;
  height: 2px;
  background: -webkit-linear-gradient(left, transparent, #03e9f4);
  left: -100%;
  top: 0px;
  animation: line1 1s linear infinite;
}
@keyframes line1 {
  50%,
  100% {
    left: 100%;
  }
}
.btn > span:nth-child(2) {
  width: 2px;
  height: 100%;
  background: -webkit-linear-gradient(top, transparent, #03e9f4);
  right: 0%;
  top: -100%;
  animation: line2 1s 0.25s linear infinite;
}
@keyframes line2 {
  50%,
  100% {
    top: 100%;
  }
}
.btn > span:nth-child(3) {
  width: 100%;
  height: 2px;
  background: -webkit-linear-gradient(left, #03e9f4, transparent);
  left: 100%;
  bottom: 0;
  animation: line3 1s 0.5s linear infinite;
}
@keyframes line3 {
  50%,
  100% {
    left: -100%;
  }
}
.btn > span:nth-child(4) {
  width: 2px;
  height: 100%;
  background: -webkit-linear-gradient(top, #03e9f4, transparent);
  left: 0px;
  top: 100%;
  animation: line4 1s 0.75s linear infinite;
}
@keyframes line4 {
  50%,
  100% {
    top: -100%;
  }
}

/* btn2 炫光 */
.btn2 {
  margin-left: 50px;
  height: 50px;
  width: 100px;
  color: #fff;
  position: relative;
  text-transform: uppercase;
  letter-spacing: 2px;
  background: linear-gradient(90deg, #03a9f4, #37e972, #f874f2, #03a9f4);
  border-radius: 5px;
  background-size: 400%;
  z-index: 1;
}
.btn2::before {
  content: '';
  position: absolute;
  left: -5px;
  right: -5px;
  top: -5px;
  bottom: -5px;
  /* border: 1px solid red; */
  background: linear-gradient(90deg, #03a9f4, #37e972, #f874f2, #03a9f4);
  background-size: 400%;
  border-radius: 5px;
  filter: blur(10px);
  z-index: -1;
}
.btn2:hover::before {
  animation: sun 5s infinite;
}
.btn2:hover {
  animation: sun 5s infinite;
  cursor: pointer;
}
@keyframes sun {
  100% {
    background-position: -400% 0;
  }
}
.watch p{
  color: rgb(155, 155, 155);
  margin-top: 50px;
  font-size: 13px;
}
</style>
