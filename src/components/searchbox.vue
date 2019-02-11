<template>
  <div class="search-box">
    <input @click='toSearch' type="text" placeholder="搜索">
    <icon type="search" size="12"></icon>
  </div>
</template>
<script>
export default {
  methods: {
    toSearch(){
      wx.navigateTo({ url: '/pages/search/main' });
    }
  },
};
</script>
<style lang='scss'>
$maincolor: #ff2d4a;
.search-box {
  background-color: $maincolor;
  box-sizing: border-box;
  padding: 20rpx 16rpx;
  position: fixed;
  z-index: 999;
  top: 0;
  left: 0;
  width: 100%;
  input {
    box-sizing: border-box;
    height: 60rpx;
    width: 100%;
    padding-left: 376rpx;
    background-color: #fff;
    font-size: 24rpx;
    border-radius: 10rpx;
  }
  icon {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>