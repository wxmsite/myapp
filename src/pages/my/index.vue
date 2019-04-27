<template>
  <view>
    <view class="top">
      <view v-if="status" class="avatar">
        <open-data class="avatar-img" type="userAvatarUrl"></open-data>
      </view>
      <view v-else class="avatar">
        <img class="avatar-img" src="../../../static/images/avatar.png">
      </view>
      <view v-if="status" class="nickname">
        <button class="btn">
          <open-data class="nickname-text" type="userNickName"></open-data>
        </button>
      </view>
      <view v-else class="nickname">
        <button class="btn" open-type="getUserInfo" @getuserinfo="onGotUserInfo">点击登录</button>
      </view>
    </view>

    <view class="menu">
      <view class="menu-item" @click="uploadFile">
        <view>
          <img class="menu-item-icon" src="../../../static/images/upload-file.svg">
        </view>
        <span class="menu-item-content">上传文件</span>
      </view>
      <view class="modal">
        <modal :hidden="hiddenmodalput" title="请输入文件名" @confirm="confirm" @cancel="cancel">
          <view>
            <input class="bookName" v-model="bookName" type="text" @input="getbookName">
          </view>
        </modal>
      </view>
      <view class="menu-item" @click="collection">
        <view class="menu-item-icon">
          <img class="menu-item-icon" src="../../../static/images/collect.svg">
        </view>
        <span class="menu-item-content">我的收藏</span>
      </view>
      <view class="menu-item" @click="explanation">
        <view class="menu-item-icon">
          <img class="menu-item-icon" src="../../../static/images/explanation.svg">
        </view>
        <span class="menu-item-content">功能说明</span>
      </view>
      <view class="menu-item" @click="problem">
        <view class="menu-item-icon">
          <img class="menu-item-icon" src="../../../static/images/problem.svg">
        </view>
        <span class="menu-item-content">待解决问题</span>
      </view>
      <view class="menu-item" @click="feedback">
        <view class="menu-item-icon">
          <img class="menu-item-icon" src="../../../static/images/feedback.svg">
        </view>
        <span class="menu-item-content">意见反馈</span>
      </view>
    </view>
    <view>
      <button class @click="logout">退出登录</button>
    </view>
  </view>
</template>


<script>
import card from "@/components/card";

export default {
  components: {
    card
  },

  data() {
    return {
      hiddenmodalput: true,
      bookName: "",
      nickName: "",
      status: false,

      logs: [],
      form: {
        account: "",
        password: ""
      }
    };
  },
  methods: {
    getbookName(e) {
      this.bookName = e.target.value;
    },
    cancel() {
      this.hiddenmodalput = true;
      console.log(this.hiddenmodalput);
    },
    confirm() {
      this.hiddenmodalput = true;
      //此处需要声明一个局部变量，不然author那里传不进去
      var nickName = this.nickName;
      var bookName = this.bookName;
      wx.chooseImage({
        count: 1,
        sizeType: ["original"],
        sourceType: ["album"],
        success: function(res) {
          var filePath = res.tempFilePaths[0];
          wx.showLoading({
            title: "上传中"
          });
          console.log("filepath:" + filePath);

          const cloudPath = bookName;
          wx.cloud.init();
          wx.cloud.uploadFile({
            cloudPath,
            filePath,
            success: res => {
              wx.cloud.callFunction({
                name: "setInfo",
                data: {
                  author: nickName,
                  bookName: bookName,
                  imgName: cloudPath,
                  like: 0,
                  dislike: 0
                },
                success: function(res) {
                  console.log(res);
                },
                fail: function(res) {
                  console.log(res);
                }
              });

              console.log("[上传文件] 成功：", res);

              app.globalData.fileID = res.fileID;
              app.globalData.cloudPath = cloudPath;
              app.globalData.imagePath = filePath;

              that.setData({
                fileID: res.fileID,
                cloudPath: cloudPath,
                imagePath: filePath
              });

              console.log(that.data);
              wx.showToast({
                title: "成功",
                icon: "success",
                duration: 2000
              });
            },
            fail: e => {
              console.error("[上传文件] 失败：", e);
              wx.showToast({
                icon: "none",
                title: "上传失败"
              });
            },
            complete: () => {
              wx.hideLoading();
            }
          });
        },
        fail: e => {
          console.error(e);
        }
      });
      this.bookName = "";
    },

    bookName: function(e) {
      this.setData({
        bookName: e.detail.value
      });
    },

    uploadFile() {
      //弹出modal，输入文件名
      this.hiddenmodalput = false;
    },
    collection() {
      wx.showToast({
        title: "计划中",
        icon: "success",
        duration: 1000
      });
    },
    explanation() {
      /*  wx.navigateTo({
        url: "../explanation/main"
      }); */
      wx.showToast({
        title: "待实现",
        icon: "success",
        duration: 1000
      });
    },
    problem() {
      wx.navigateTo({
        url: "../problem/main"
      });
    },
    feedback() {
      wx.showToast({
        title: "计划中",
        icon: "success",
        duration: 1000
      });
    },
    logout() {
      console.log("------>logout");
      try {
        wx.removeStorageSync("userInfo");
      } catch (e) {
        console.log(e);
      }
      this.status = false;
    },
    onGotUserInfo(e) {
      console.log("------>login");
      wx.setStorageSync("userInfo", e.mp.detail.userInfo);
      this.nickName = wx.getStorageSync("userInfo").nickName;
      this.status = true;
      console.log(this.nickName);
    }
  },
  onShow() {
    console.log("------>login show");
    if (wx.getStorageSync("userInfo")) {
      this.nickName = wx.getStorageSync("userInfo").nickName;
      this.status = true;
    }
  }
};
</script>

<style lang='scss' scoped>
@import "./style";
</style>
