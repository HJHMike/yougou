<template>
  <div class='cart-container'>
    <!-- 空车 -->
    <div class="empty">
      <i class='iconfont icon-gouwuche'></i>
      <p>你还没有添加任何商品</p>
    </div>
    <!-- 有商品 -->
    <div class="full"></div>
  </div>
</template>

<script>


export default {
  
}
</script>

<style lang='scss'>
page{
  height: 100%;
}

.cart-container{
  height: 100%;
  .empty{
    height: 100%;
    text-align: center;
    padding-top: 100rpx;
    box-sizing: border-box;
    i{
      display: block;
      margin: 0 auto;
      font-size: 100rpx;
      width: 180rpx;
      height: 180rpx;
      line-height: 180rpx;
      border-radius: 50%;
      background: linear-gradient(to bottom,#e48985,#c35c53);
      color: #dfca4d;
    }
    p{
      margin-top: 20rpx;
      font-size: 30rpx;
      // font-weight: 700;
    }
  }
}
</style>
