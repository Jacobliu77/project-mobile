
<template>
  <div class="home-container">
    <!-- 导航栏 -->
    <van-nav-bar title="首页" />
    <!-- /导航栏 -->

    <!-- 频道列表 -->
    <!--
      tab 组件
        v-model="active" 控制被激活的标签
        title 标签标题
     -->
    <van-tabs v-model="active">
     <van-tab
        :title="channel.name"
        v-for="channel in userChannels"
        :key="channel.id"
      >
        <!-- 文章列表 -->
         <article-list :channel="channel" />
        <!-- /文章列表 -->
      </van-tab>
    </van-tabs>
    <!-- /频道列表 -->
  </div>
</template>

<script>
import { getUserChannels } from '@/api/user'
import ArticleList from './components/article-list'
export default {
  components: {
    ArticleList
  },
  data () {
    return {
      active: 0,
      userChannels: []
    }
  },
  methods: {
    async loadUserChannels () {
      const { data } = await getUserChannels()
      this.userChannels = data.data.channels
    }
  },
  created () {
    this.loadUserChannels()
  }
}
</script>

<style scoped></style>
