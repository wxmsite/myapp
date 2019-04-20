<template>
  <view class="problem">
    <view class="applet">
      <img class="applet-code" src="../../../static/images/appletCode.jpg">
    </view>
    <view
      class="content"
      v-for="(item, index) in problemList"
      :key="index"
    >{{item.index}} {{item.value}}</view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      problemList: [
        { index: 1, value: "登陆时闪烁" },
        { index: 2, value: "界面UI待优化" },
        { index: 3, value: "更多功能待开发" }
      ]
    };
  }
};
</script>

<style lang='scss' scoped>
@import "./style";
</style>