<template>
  <view class="mindmap-card" @click="getBook">
    <view class="card-thum">
      <img src="../../static/images/avatar.png">
    </view>
    <view class="card-name">
      <view class="title">人类简史</view>
      <view class="author">啦啦啦</view>
      <view class="features">
        <view>
          <img src="../../static/images/like.svg">
        </view>
        <view>
          <img src="../../static/images/dislike.svg">
        </view>
        <view>
          <img src="../../static/images/collect.svg">
        </view>
      </view>
    </view>
  </view>
</template>
<script>
export default {
  methods: {
    getBook() {}
  }
};
</script>
<style lang="less">
.mindmap-card {
  display: flex;
  .card-thum {
  }
  .card-name {
    flex-direction: column;
    .title {
    }
    .author {
    }
    .features {
      flex-direction: row;
      justify-content: flex-start;
      align-items: center;
    }
  }
}
</style>

