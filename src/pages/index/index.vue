<template>
  <view class="minding">
    <view class="search" @click="search">输入书籍</view>
    <view class="content">
      <mindmap v-for="(item,index) in mindmapList" :key="index" :book="item"></mindmap>
    </view>
  </view>
</template>
<script>
import mindmap from "@/components/mindmap.vue";
export default {
  components: {
    mindmap
  },
  data() {
    return {
      mindmapList: [
        { author: "123", title: "book1" },
        { author: "234", title: "book2" },
        { author: "345", title: "book3" }
      ]
    };
  },
  methods: {
    init() {
      getMindingMapList();
    },
    search() {
      wx.showToast({
        title: "待实现",
        icon: "success",
        duration: 1000
      });
    },
    getMindingMapList() {}
  }
};
</script>
<style lang='scss' scoped>
@import "./style";
</style>


