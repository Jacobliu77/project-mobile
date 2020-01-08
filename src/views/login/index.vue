<template>

<div class="login-container">
    <van-nav-bar
        title="登录"
        left-arrow
    />
    <van-cell-group>
<van-row type="flex" align="center">
 <van-col span="4">
<i class="iconfont icon-mobile" ></i>
</van-col>
<van-col span="20">
  <van-field
    v-model="user.mobile"
    required
    clearable
    placeholder="请输入手机号"
  />
  </van-col>
  </van-row>
  <van-row type="flex" align="center">
 <van-col span="4">
  <i class="iconfont icon-ecurityCode"></i>
  </van-col>
<van-col span="20">
  <van-field
    v-model="user.code"
    placeholder="请输入验证码"
    required
  >
  <van-count-down
  v-if="isCountDownShow"
  slot="button"
  format=" ss 秒后可重新发送"
  :time="1000*60" />
   <van-button
    v-else
    slot="button"
    size="small"
    type="primary"
    @click="onsmscode">发送验证码</van-button>
  </van-field>
   </van-col>
  </van-row>
</van-cell-group>
<div class="login-btn-container">
<van-button class="login-btn" @click="onLogin" type="info">登录</van-button>
</div>
</div>
</template>

<script>
// 引入请求模块
import { login, smscode } from '@/api/user'

export default {
  name: 'LoginPage',
  data () {
    return {
      user: {
        mobile: '',
        code: ''
      },
      isCountDownShow: false
    }
  },
  methods: {
    async onLogin () {
      this.$toast.loading({
        duration: 0, // 持续时间，0表示持续展示不停止
        forbidClick: true, // 是否禁止背景点击
        message: '登录中...' // 提示消息
      })
      try {
        const res = login(this.user)
        console.log('登录成功', res)
        this.$toast.success('登录成功')
      } catch (err) {
        console.log('登录失败', err)
        this.$toast.fail('登录失败，手机号或验证码错误')
      }
    },
    async onsmscode () {
      const { mobile } = this.user
      try {
        // 显示倒计时
        this.isCountDownShow = true
        // 发送
        await smscode(mobile)
      } catch (err) {
        // 发送失败，关闭倒计时
        this.isCountDownShow = false
        if (err.response.status === 429) {
          this.$toast('请勿频繁发送')
          return
        }
        this.$toast('发送失败')
      }
    }
  }
}
</script>

<style lang='less' scoped>
  .login-container {
  .login-btn-container {
    padding: 20px;
    .login-btn {
      background-color: #6db4fb;
      width: 100%;
    }
  }
  .iconfont{
    font-size: 24px
  }
}
</style>
