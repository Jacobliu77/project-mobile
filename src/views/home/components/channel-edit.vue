<template>

<div class="channel-edit">
    <van-cell title="我的频道" :border="false" style="text-align: left;">
      <van-button size="mini" round type="danger" plain>编辑</van-button>
    </van-cell>
    <van-grid :gutter="10">
        <van-grid-item
          v-for="channel in uchannels"
          :key="channel.id"
          :text="channel.name"
        />
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
      allChannels: [] // 所有频道
    }
  },
  methods: {
    async loadAllChannels () {
      const { data } = await getAllChannels()
      this.allChannels = data.data.channels
    },
    onAdd (channel) {
      this.uchannels.push(channel)
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

    }
</style>
