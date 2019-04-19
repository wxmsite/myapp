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
        <button class="btn" open-type="getUserInfo" @getuserinfo="onGotUserInfo">微信登录</button>
      </view>
    </view>

    <view class="menu">
      <view class="menu_item" @click="uploadFile">
        <view>
          <img class="menu_item_icon" src="../../../static/images/upload-file.svg">
        </view>
        <span class="menu_item_content">上传文件</span>
      </view>
      <view class="menu_item">
        <view class="menu_item_icon">
          <img class="menu_item_icon" src="../../../static/images/explanation.svg">
        </view>
        <span class="menu_item_content">一些说明</span>
      </view>
      <view class="menu_item">
        <view class="menu_item_icon">
          <img class="menu_item_icon" src="../../../static/images/feedback.svg">
        </view>
        <span class="menu_item_content">意见反馈</span>
      </view>
    </view>
    <!-- <view>
      <button class @click="logout">退出登录</button>
    </view>-->
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
      userInfo: "",
      status: false,
      isPassword: true,
      logs: [],
      form: {
        account: "",
        password: ""
      }
    };
  },
  methods: {
    uploadFile() {
      wx.chooseImage({
        count: 1, // 默认9
        sizeType: ["original"], // 可以指定是原图还是压缩图，默认二者都有
        sourceType: ["album"], // 可以指定来源是相册还是相机，默认二者都有
        success: function(res) {
          // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
          var filePath = res.tempFilePaths[0];
          wx.showLoading({
            title: "上传中"
          });
          console.log(filePath);
          const cloudPath =filePath.match(/\.[^.]+.png/)[0];
          console.log(cloudPath);
          wx.cloud.init();
          wx.cloud.uploadFile({
            cloudPath,
            filePath,
            success: res => {
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
              wx.navigateTo({
                url: "../storageConsole/storageConsole"
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
    },
    logout() {
      console.log("------>logout");
      try {
        wx.removeStorageSync("userInfo");
      } catch (e) {
        // Do something when catch error
        wx.switchTab({
          url: "../tabBar/course/main"
        });
        console.log(e);
      }
      this.status = false;
    },
    onGotUserInfo(e) {
      console.log("------>login");
      wx.setStorageSync("userInfo", e.mp.detail.userInfo);
      wx.switchTab({
        url: "../tabBar/index/main"
      });
      this.status = true;
    }
  },
  onShow() {
    console.log("------>login show");

    if (wx.getStorageSync("access_token")) {
      wx.switchTab({
        url: "../tabBar/course/main"
      });
    } else {
      this.status = true;
    }
  },

  onReady() {
    console.log("------>login ready");
  },
  OnHide() {
    console.log("onhide");
    this.status = false;
  },
  onUnload() {
    console.log("onupload");
    this.status = false;
  },
  onShareAppMessage: function(res) {
    if (res.from === "button") {
      // 来自页面内转发按钮
      console.log(res.target);
    }
    return {
      title: "meedu",
      path: "pages/login/main"
    };
  },
  _getUserInfo(data) {
    this.$http.user.getUserInfo().then(res => {
      this.userInfo = res.data;
    });
  }
};
</script>

<style lang='scss' scoped>
@import "./style";
</style>
