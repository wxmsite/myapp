<template>
  <view class="minding">
    <view class="search" @click="search">搜索</view>
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
      mindmapList: [],
    
    };
  },
  methods: {
    search() {
      wx.navigateTo({
        url: "../search/main"
      });
      
    },
    //get mindmapList on main page
    getMindingMapList() {
      wx.cloud.init();
      wx.cloud.callFunction({
        name: "getBookList",
        complete: res => {
          this.mindmapList.splice(0);
          for (var l = 0; l < res.result.data.length; l++) {
            this.mindmapList.push({
              author: res.result.data[l].author,
              title: res.result.data[l].bookName,
              imgName: res.result.data[l].imgName,
              id: res.result.data[l]._id
            });
          }
        }
      });
    }
  },
  onShow() {
    console.log("index show ");
    this.getMindingMapList();
  }
};
</script>
<style lang='scss' scoped>
@import "./style";
</style>


