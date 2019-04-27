<template>
  <view class="problem">
    <view class="applet">
      <img class="applet-code" src="../../../static/images/appletCode.jpg">
    </view>
    <view
      class="content"
      v-for="(item, index) in problemList"
      :key="index"
    >{{index+1}} {{item.value}}</view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      problemList: [
        { value: "登陆时闪烁/未能点击头像登录" },
        { value: "首页缩略图问题" },
        { value: "更多功能待开发" },
        { value: "界面UI待优化" }
      ]
    };
  }
};
</script>

<style lang='scss' scoped>
@import "./style";
</style>