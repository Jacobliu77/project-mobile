<template>
  <div id="app">
    <!-- 一级路由视图区 -->
    <van-cell title="选择单个日期" :value="date" @click="show = true" />
    <van-calendar v-model="show" @confirm="onConfirm" />
    <router-view />
  </div>
</template>
<script>
export default {
  name: 'App',
  data () {
    return {
      date: '',
      show: false
    }
  },

  methods: {

  }
}
</script>
<style lang="less" scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
