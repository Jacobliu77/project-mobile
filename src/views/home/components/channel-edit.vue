<template>

<div class="channel-edit">
    <van-cell title="我的频道" :border="false" style="text-align: left;">
      <van-button
      size="mini"
      round
      type="danger"
      plain
      @click="isEditShow = !isEditShow"
      >{{isEditShow?"完成":"编辑"}}</van-button>
    </van-cell>
    <van-grid :gutter="10">
        <van-grid-item
          v-for="(channel,index) in uchannels"
          :key="channel.id"
          :text="channel.name"
           @click="onUserChannelClick(index)"
        >
         <van-icon class="close-icon" v-show="isEditShow" slot="icon" name="close" />
        </van-grid-item>
    </van-grid>
     <van-cell title="所有频道" :border="false" style="text-align: left;">
    </van-cell>
    <van-grid :gutter="10">
        <van-grid-item
          v-for="channel in remainingChannels"
          :key="channel.id"
          :text="channel.name"
          @click="onAdd(channel)"
        />
    </van-grid>
</div>
</template>

<script>
import { getAllChannels } from '@/api/channel'
export default {
  name: 'ChannelEdit',
  props: {
    uchannels: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      allChannels: [], // 所有频道
      isEditShow: false
    }
  },
  methods: {
    async loadAllChannels () {
      const { data } = await getAllChannels()
      this.allChannels = data.data.channels
    },
    onAdd (channel) {
      this.uchannels.push(channel)
    },
    onUserChannelClick (index) {
      if (this.isEditShow) {
        this.uchannels.splice(index, 1)
      } else {
        // 如果是非编辑状态，则切换频道
        this.$emit('input', index) // 修改激活的标签
        this.$emit('close') // 关闭弹层
      }
    }
  },
  created () {
    this.loadAllChannels()
  },
  computed: {
    remainingChannels () {
      const { allChannels, uchannels } = this
      // 剩余频道 = 所有频道 - 我的频道
      const channels = []
      // 遍历所有频道
      allChannels.forEach(item => {
        // 如果我的频道中不包含当前被遍历的频道，则要
        if (!uchannels.find(c => c.id === item.id)) {
          channels.push(item)
        }
      })
      return channels
    }
  }
}

</script>

<style lang="less" scoped>
    .channel-edit {
        padding: 0;
        margin: 50px 0;
        .van-grid{
             .van-grid-item__content--center{
        background-color: #f4f5f6;
        }
      }
      ::v-deep .van-grid-item__content {
    position: relative;
    .van-grid-item__icon-wrapper {
      position: absolute;
      top: -18px;
      right: -5px;
      .close-icon {
        font-size: 16px;
      }
    }
  }
    }
</style>
