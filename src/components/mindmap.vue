<template>
  <view class="mindmap" @click="getBook">
    <view class="card-thum">
      <img src="../../static/images/like.svg">
    </view>
    <view class="card-name">
      <view class="title">{{book.title}}</view>
      <view class="author">{{book.author}}</view>
      <view class="features">
        <view @click="like">
          <img class="img" src="../../static/images/like.svg">
        </view>
        <view @click="dislike">
          <img class="img" src="../../static/images/dislike.svg">
        </view>
        <view>
          <img class="img" src="../../static/images/collect.svg">
        </view>
        <view @click="download">
          <img class="img" src="../../static/images/download.svg">
        </view>
      </view>
    </view>
  </view>
</template>
<script>
export default {
  props: {
    book: {
      type: String,
      default() {
        return {};
      }
    }
  },
  methods: {
    getBook() {},
    download() {
      wx.downloadFile({
        url:
          "https://6d79-myapp-046e4f-1259071453.tcb.qcloud.la/" +
          this.book.imgName,
        success: function(res) {
          // 只要服务器有响应数据，就会把响应内容写入文件并进入 success 回调，业务需要自行判断是否下载到了想要的内容
          if (res.statusCode === 200) {
            var tempFilePaths = res.tempFilePath;
            wx.saveImageToPhotosAlbum({
              filePath: tempFilePaths,
              success(res) {
                wx.showToast({
                  title: "保存成功",
                  icon: "success",
                  duration: 2000
                });
              },
              fail(res) {
                wx.showToast({
                  title: "保存失败",
                  icon: "success",
                  duration: 2000
                });
              }
            });
          }
        }
      });
    },
    like() {
      
    },
    dislike() {}
  }
};
</script>
<style lang="less">
.mindmap {
  display: flex;
  width: 100%;
  margin-bottom: 20px;
  border-bottom: 1px solid rgb(240, 230, 230);
  .card-thum {
    display: flex;
    align-items: center;
    width: 120rpx;
    height: 120px;
    margin-left: 20rpx;
  }
  .card-name {
    display: flex;
    flex: 1;
    padding-left: 40rpx;
    flex-direction: column;
    justify-content: space-between;

    .title {
      font-size: 16px;
    }
    .author {
      font-size: 12px;
      color: rgb(117, 104, 104);
    }
    .features {
      display: flex;
      flex-direction: row;
      margin-bottom: 10px;
      justify-content: flex-end;
      margin-right: 40rpx;
      .img {
        width: 40rpx;
        height: 40rpx;
        display: inline-block;
        padding-left: 20rpx;
      }
    }
  }
}
</style>


