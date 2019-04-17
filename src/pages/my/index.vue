<template>
  <view>
    <view class="top">
      <view class="organ-name">
        <view v-if="userInfo">{{userInfo.nick_name}}</view>
        <open-data type="userNickName"></open-data>
      </view>
      <view class="avatar">
        <img v-if="userInfo" class="avavtao-img" :src="userInfo.avatar" alt mode="widthFix">
        <open-data v-else class="avavtao-img" type="userAvatarUrl"></open-data>
      </view>
    </view>

    <button class="btn" open-type="getUserInfo" @getuserinfo="onGotUserInfo">微信授权登录</button>

    <view class="menu">
      <view class="menu_item">
        <view>
          <img class="menu_item_icon" src="../../../static/images/user2.png">
        </view>
        <span class="menu_item_content">menu1</span>
      </view>
      <view class="menu_item">
        <view class="menu_item_icon">
          <img class="menu_item_icon" src="../../../static/images/user2.png">
        </view>
        <span class="menu_item_content">menu2</span>
      </view>
      <view class="menu_item">
        <view class="menu_item_icon">
          <img class="menu_item_icon" src="../../../static/images/user2.png">
        </view>
        <span class="menu_item_content">menu3</span>
      </view>
    </view>
    <view>
      <button class="logout-btn" v-on:click.native="logout">退出登录</button>
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
      userInfo: "",
      isHidden: false,
      isPassword: true,
      logs: [],
      form: {
        account: "",
        password: ""
      }
    };
  },
  methods: {
    init() {
      this._getUserInfo();
    },
    onGotUserInfo(e) {
      console.log("------>login");
      wx.setStorageSync("userInfo", e.mp.detail.userInfo);
      wx.switchTab({
        url: "../my/main"
      });
    }
  },
  onShow() {
    console.log("------>login show");
    if (wx.getStorageSync("access_token")) {
      wx.switchTab({
        url: "../tabBar/course/main"
      });
    } else {
      this.isHidden = true;
    }
  },
  logout() {
    console.log("------>logout");
    try {
      wx.clearStorageSync();
      this.isHidden = true;
    } catch (e) {
      // Do something when catch error
      wx.switchTab({
        url: "../tabBar/course/main"
      });
      console.log(e);
    }
    wx.reLaunch({
      url: "../../login/main"
    });
  },
  onReady() {
    console.log("------>login ready");
  },
  OnHide() {
    console.log("onhide");
    this.isHidden = false;
  },
  onUnload() {
    console.log("onupload");
    this.isHidden = false;
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
