<template>
  <div>
    <img src="../../assets/img/wave.png" class="wave">
    <div class="container">
      <div class="img">
        <img src="../../assets/img/bg.svg" alt="">
      </div>
      <div class="login-content">
        <form ref="loginForm" :model="loginForm" @submit.prevent="login">
          <img src="../../assets/img/avatar.svg" alt="">
          <h2 class="title">欢迎</h2>
          <div class="input-div one">
            <div class="i">
              <i class="fas fa-user" />
            </div>
            <div class="div">
              <h5>用户名</h5>
              <input v-model="loginForm.username" type="text" class="input">
            </div>
          </div>
          <div class="input-div pass">
            <div class="i">
              <i class="fas fa-lock" />
            </div>
            <div class="div">
              <h5>密码</h5>
              <input v-model="loginForm.password" type="password" class="input">
            </div>
          </div>
          <a href="#">忘记密码?</a>
          <button type="submit" class="btn">登录</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'MainLogin',
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '20021011'
      },
      loading: false,
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },

  methods: {
    login() {
      // Add your login logic here
      const loginData = {
        username: this.loginForm.username,
        password: this.loginForm.password
      }
      console.log(loginData)
      // 发送POST请求
      axios.post('http://localhost:8080/login', loginData)
        .then(response => {
          // 处理登录成功的响应
          console.log(response)
          console.log(response.data.code)
          if (response.data.code === 1) {
            sessionStorage.setItem('Authorization', response.data.data)

            console.log(sessionStorage.Authorization)
            // this.$router.push('/')
            this.loading = true
            this.$store.dispatch('user/login', this.loginForm).then(() => {
              this.$router.push({ path: this.redirect || '/' })
              this.loading = false
            }).catch(() => {
              this.loading = false
            })
          } else {
            this.$message.error('用户名或密码错误!')
          }
        })
        .catch(error => {
          // 处理登录失败的响应
          console.error(error)
        })
    }
  }
}
</script>
<style scoped>
/* 初始化 */
*{
  padding: 0;
  margin: 0;
  /* 标准盒子 */
  box-sizing: border-box;
}

body{
  /* 引入字体 */
  font-family: "Poppins",sans-serif;
  /* 溢出隐藏 */
  overflow: hidden;
}

.wave{
  position: fixed;
  bottom: 0;
  left: 0;
  height: 100%;
  /* 让他当作一个背景 */
  z-index: -1;
}

.container{
  width: 100vw;
  height: 100vh;
  /* 网格布局 */
  display: grid;
  grid-template-columns: repeat(2,1fr);
  grid-gap: 7rem;
  padding: 0 2rem;
}

.img{
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

.login-content{
  display: flex;
  justify-content: flex-start;
  align-items: center;
  /* 文字居中 */
  text-align: center;
}

.img img{
  width: 500px;
}

form{
  width: 360px;
}

.login-content img{
  height: 100px;
}

.login-content h2{
  margin: 15px 0;
  color: #333;
  text-transform: uppercase;
  font-size: 2.9rem;
}

.login-content .input-div{
  /* 相对定位 */
  position: relative;
  display: grid;
  grid-template-columns: 7% 93%;
  margin: 25px 0;
  padding: 5px 0;
  border-bottom: 2px solid #d9d9d9 ;
}

.login-content .input-div.one{
  margin-top: 0;
}

.i{
  color:#d9d9d9 ;
  /* 弹性布局 水平 垂直居中 */
  display: flex;
  justify-content: center;
  align-items: center;
}

.i  i{
  transition: .3s;
}

.input-div > div{
  /* 相对定位 */
  position: relative;
  height: 45px;
}

.input-div > div > h5{
  /* 绝对定位 */
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  color: #999;
  font-size: 18px;
  transition: .3s;
}

.input-div::before , .input-div::after{
  content: "";
  /* 绝对定位 */
  position: absolute;
  bottom: -2px;
  width: 0%;
  height: 2px;
  background-color: #38d39f;
  transition: .4s;
}

.input-div::before{
  right: 50%;
}

.input-div::after{
  left: 50%;
}

.input-div.focus::before , .input-div.focus::after{
  width: 50%;
}

.input-div.focus > div >h5{
  top: -5px;
  font-size: 15px;
}

.input-div.focus > .i > i{
  color: #38d39f;
}

.input-div > div > input{
  /* 绝对定位 */
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  border: none;
  /* 点击输入框的边框也消失 */
  outline: none;
  padding: 0.5rem 0.7rem;
  background: none;
  font-size: 1.2rem;
  color: #555;
  font-family: 'position',sans-serif;
}
.input-div.pass{
  margin-bottom: 4px;
}
a{
  display: block;
  text-align: right;
  /* 下划线消失 */
  text-decoration: none;
  color: #999;
  font-size: 0.9rem;
  transition: .3s;
}
a:hover{
  color: #38d39f;
}
.btn{
  display: block;
  width: 100%;
  height: 50px;
  border-radius: 25px;
  outline: none;
  border: none;
  background-image: linear-gradient(to right,#32be8f,#32b39f,#32be8f);
  background-size: 200%;
  font-size: 1.2rem;
  color: #fff;
  font-family: 'Poppins',sans-serif;
  text-transform: uppercase;
  margin: 1rem 0;
  /* 鼠标放上变小手 */
  cursor: pointer;
  transition: .5s;
}

.btn:hover{
  background-position: right;
}

@media  screen and (max-width: 1050px){
  .container{
    grid-gap: 5rem;
  }
}

@media  screen and (max-width: 1000px){
  form{
    width: 290px;
  }
  .login-content h2{
    font-size: 2.4rem;
    margin: 8px 0;
  }
  .img img{
    width: 400px;
  }
}

@media  screen and (max-width: 900px){
  .container{
    grid-template-columns: 1fr;
  }
  .img{
    display: none;
  }
  .wave{
    display: none;
  }
  .login-content{
    justify-content: center;
  }
}
</style>

