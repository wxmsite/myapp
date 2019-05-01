<template>
  <view class="search">
    <input class="search_input" v-model="searchName" @confirm="confirm" placeholder="请输入搜索内容">
    <view class="recommend">
      <p class="recommend_title">书籍推荐</p>
      <span
        class="recommend_content"
        @click="search(item)"
        v-for="(item, index) in recommendList"
        :key="index"
      >{{item}}</span>
    </view>
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
      recommendList: ["穷查理宝典", "大败局", "富兰克林自传", "刻意练习"],
      mindmapList: [],
      searchName: ""
    };
  },
  methods: {
    confirm() {
      this.search(this.searchName);
    },
    search(s) {
      wx.cloud.init();
      wx.cloud.callFunction({
        name: "searchBook",
        data: {
          bookName: s
        },
        complete: res => {
          this.mindmapList.splice(0);
          for (var l = 0; l < res.result.data.length; l++) {
            this.mindmapList.push({
              author: res.result.data[l].author,
              title: res.result.data[l].bookName,
              bookName: res.result.data[l].bookName,
              imgName: res.result.data[l].imgName,
              id: res.result.data[l]._id
            });
          }
          //clear input value
          this.searchName = "";
        }
      });
    },
    //get hot topic here ,but get all now
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