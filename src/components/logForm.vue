<template>
  <div class="login-form">    <!--样式写在父组件中-->
    <div class="g-form">
      <div class="g-form-line">
        <span class="g-form-label">用户名：</span>
        <div class="g-form-input">
          <input type="text" v-model="usernameModel" placeholder="请输入用户名">
        </div>
        <span class="g-form-error">{{ userErrors.errorText }}</span>
      </div>

      <!--密码-->
      <div class="g-form-line">
        <span class="g-form-label">密码：</span>
        <div class="g-form-input">
          <input type="password" v-model="passwordModel" placeholder="请输入密码">
        </div>
        <span class="g-form-error">{{ passwordErrors.errorText}}</span>
      </div>
      <div class="g-form-line">
        <div class="g-form-btn">
          <a class="button" @click="onLogin">登录</a>
        </div>
      </div>
      <p class="g-form-error">{{errorText}}</p>
    </div>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        usernameModel: '',
        passwordModel: '',
        errorText: ''
      }
    },
    computed: {
      userErrors () {
        let errorText, status
        if (!/@/g.test(this.usernameModel)) {   // 只想说this的功能好强大
          status = false
          errorText = '不包含@'
        } else {
          status = true
          errorText = ''
        }
        if (!this.userFlag) {
          errorText = ''
          this.userFlag = true
        }
        return {
          status,
          errorText
        }
      },
      passwordErrors () {
        let errorText, status
        if (!/^\w{6,10}$/g.test(this.passwordModel)) {
          status = false
          errorText = '密码必须是6-10位'
        } else {
          status = true
          errorText = ''
        }
        if (!this.passwordFlag) {
          errorText = ''
          this.passwordFlag = true
        }
        return {
          status,
          errorText
        }
      }
    },
    methods: {
      onLogin () {
        if (!this.userErrors.status || !this.passwordErrors.status) { // 如果status为false的话，那么就是没有验证通过的咯
             this.errorText = '部分选项未通过'
        } else {
          this.errorText = ''
          this.$http.get('api/login')
            .then((res) => {
                this.$emit('has-log', res.data) // this.emit触发父组件v-on：has-log="onSuccessLog"   监听的时间
          }, (error) => {
            console.log(error)
          })
        }
      }
    }
  }
</script>
<style>
</style>
