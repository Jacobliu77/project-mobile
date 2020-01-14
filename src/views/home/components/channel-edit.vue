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
          v-for="value in 8"
          :key="value"
          text="文字"
        />
    </van-grid>

</div>

</template>

<script>

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

    }
  },
  methods: {
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
