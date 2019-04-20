<template>
  <view>
    <view class="minding">
      <view class="search" @click="search">输入书籍</view>
    </view>
    <view>
      <mindmap-card></mindmap-card>
    </view>
  </view>
</template>
<script>
import mindmap from "@/components/mindmap.vue";
export default {
  methods: {
    init() {
      getMindingMap();
    },
    search() {
      wx.showToast({
        title: "待实现",
        icon: "success",
        duration: 1000
      });
    },
    getMindingMap() {
      
    }
  }
};
</script>
<style lang='scss' scoped>
@import "./style";
</style>


