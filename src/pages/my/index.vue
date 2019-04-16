<template>
  <div v-if="userInfo.avatar">
    <img class="avatar" :src="userInfo.avatar">
    <div class="info">
      <text class="name">{{userInfo.nickname}}</text>
    </div>
  </div>
  <div v-else class="goLogin">
    <img class="icon" src="/static/images/ic_menu_me_pressed.png">
    <button
      v-if="canIUse"
      open-type="getUserInfo"
      @getuserinfo="goLogin"
      class="goLoginBtn"
    >点击，授权登录~</button>
  </div>
</template>


<style lang='scss' scoped>
  @import "./style";
</style>
