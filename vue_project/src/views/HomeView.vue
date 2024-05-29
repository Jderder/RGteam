<template>
  <div class="mainContainer">
    <div class="BackgroundContainer">
      <div class="leftContainer">
        欢迎使用Data-co-pilot！
      </div>
      <div class="rightContainer">
        <div class="ListContainer" v-if="!isShowRegister">
          <div class="TopTextContainer">
            登录您的帐号
          </div>
          <div class="loginInputContainer">
            <div class="loginUsernameContainer">
              <el-input v-model="username" placeholder="账号"/>
            </div>
            <div class="loginPasswordContainer">
              <el-input
                  v-model="password"
                  type="password"
                  placeholder="密码"
                  show-password
              />
            </div>
          </div>
          <div class="loginButtonContainer">
              <button class="loginButton" @click="onLogin('http://localhost:8084')">
                登录
              </button>
          </div>
          <div class="registerArea">
            <div
                @click="isShowRegisterPage"
                class="registerText">
                注册账号
            </div>
          </div>
        </div>
        <div class="ListContainer" v-if="isShowRegister">
          <div class="TopTextContainer">
            注册您的账号
          </div>
          <div class="registerInputArea">
            <div class="registerUsernameContainer">
              <el-input v-model="registerUsername" placeholder="请输入你的账号"/>
            </div>
            <div class="registerPasswordContainer">
              <el-input
                  v-model="registerPassword"
                  placeholder="请输入你的密码"
                  show-password
                  type="password"
              />
            </div>
          </div>
          <div class="registerButtonContainer">
            <button class="registerButton" @click="onRegister">
              注册账号
            </button>
          </div>
          <div class="loginArea">
            <div
              @click="isShowLoginPage"
              class="loginText"
            >
              返回登陆界面
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue';
  import { ElMessage } from 'element-plus';
  import { useRouter } from "vue-router";
  import api from '@/api'
  //用户登录信息
  const username = ref('');
  const password = ref('');
  //展示注册界面
  let isShowRegister = ref(false)
  //用户注册信息
  const registerUsername = ref('')
  const registerPassword = ref('')
  //同级路由跳转
  const router = useRouter();
  //登录方法
  const onLogin = async (url) => {
    if (username.value === '' || password.value === ''){
      ElMessage({
        showClose: true,
        message: '请输入您的账号和密码',
        type: 'warning'
      })
      return;
    }
    const result = await api.login({
      username: username.value,
      password: password.value
    })
    if (result.data.code === 200){
      ElMessage({
        showClose: true,
        message: '登录成功',
        type: 'success'
      })
      window.location.href = url
    }else if(result.data.code === 403){
      ElMessage({
        showClose: true,
        message: '您的账号或密码输入错误',
        type: 'warning'
      })
    }
  }
  //注册方法
  const onRegister = async () => {
    if (registerUsername.value === '' || registerPassword.value === ''){
      ElMessage({
        showClose: true,
        message: '账号,密码不能为空',
        type: 'warning'
      })
      return;
    }
    const result = await api.register({
      username: registerUsername.value,
      password: registerPassword.value
    })
    if (result.data.code === 200){
      ElMessage({
        showClose: true,
        message: '注册成功',
        type: 'success'
      })
      isShowRegister.value = false;
      registerUsername.value = ''
      registerPassword.value = ''
    }else if(result.data.code === 402){
      ElMessage({
        showClose: true,
        message: '该账号已存在',
        type: 'warning'
      })
    }else if(result.data.code === 500) {
      ElMessage({
        showClose: true,
        message: '注册失败',
        type: 'warning'
      })
      console.log(result.data.error)
    }
  }
  //登录注册页面转换
  const isShowRegisterPage = () => {
    isShowRegister.value = true
    username.value = ''
    password.value = ''
  }

  const isShowLoginPage = () => {
    isShowRegister.value = false
    registerUsername.value = ''
    registerPassword.value = ''
  }
</script>

<style scoped>
.mainContainer{
  width: 100%;
  height: 100%;
}
.BackgroundContainer{
  width: 100%;
  height: 100%;
  background-image: url(../img.jpg);
  display: flex;
  flex-direction: row;
  justify-content: left;
  align-items: center;
  background-size:cover;
	background-attachment: fixed;
}
.leftContainer{
  height: 100%;
  width: 50%;
  display: flex;
  font-size: 60px;
  font-weight: bold;
  font-family: '黑体','Microsoft YaHei','微软雅黑';
  color: #fafafa;
  justify-content: flex-end;
  align-items: center;
}
.rightContainer{
  height: 100%;
  width: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.ListContainer{
  width: 60%;
  height: 70%;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 0 10px 2px grey;
  display: flex;
  align-items: center;
  flex-direction: column;
  opacity: 0.9;
}
.TopTextContainer{
  width: 100%;
  height: 180px;
  font-size: 30px;
  font-family: SimHei,sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
}
.loginInputContainer{
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  flex: 1;
}
.loginUsernameContainer{
  width: 80%;
  margin-bottom: 25px;
  margin-top: 20px;
}
.loginPasswordContainer{
  width: 80%;
  margin-bottom: 10px;
}
.loginButtonContainer{
  width: 100%;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.loginButton{
  width: 65%;
  height: 40%;
  border-radius: 10px;
  background-color: dodgerblue;
  border-color: dodgerblue;
  color: white;
  transition: all 0.5s;
  cursor: pointer;
}
.loginButton:hover{
  background-color: lightskyblue;
}
.registerArea{
  width: 100%;
  height: 100px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.registerText{
  font-size: 15px;
  font-family: "Arial Narrow",serif;
  color: deepskyblue;
  text-decoration: underline;
  cursor: pointer;
}
.registerInputArea{
  width:100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  flex: 1;
}
.registerUsernameContainer{
  width: 80%;
  margin-top: 20px;
  margin-bottom: 25px;
}
.registerPasswordContainer{
  width: 80%;
  margin-bottom: 10px;
}
.registerButtonContainer {
  width: 100%;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.registerButton{
  width: 65%;
  height: 40%;
  border-radius: 10px;
  background-color: dodgerblue;
  border-color: dodgerblue;
  color: white;
  transition: all 0.5s;
  cursor: pointer;
}
.registerButton:hover{
  background-color: deepskyblue;
}
.loginArea{
  width: 100%;
  height: 100px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.loginText{
  font-size: 15px;
  font-family: "Arial Narrow",serif;
  color: deepskyblue;
  text-decoration: underline;
  cursor: pointer;
}
</style>