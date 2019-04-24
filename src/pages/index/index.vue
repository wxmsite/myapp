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
      mindmapList: []
    };
  },
  methods: {
    search() {
      wx.showToast({
        title: "待实现",
        icon: "success",
        duration: 1000
      });
    },
    //get mindmapList on main page
    getMindingMapList() {
      wx.cloud.init();
      wx.cloud.callFunction({
        name: "getBookList",
        complete: res => {
          this.mindmapList.splice(0);
          this.mindmapList.push({
            author: res.result.data[0].author,
            title: res.result.data[0].bookName,
            imgName: res.result.data[0].imgName
          });
        }
      });
    }
  },
  onShow() {
    this.getMindingMapList();
  }
};
</script>
<style lang='scss' scoped>
@import "./style";
</style>


