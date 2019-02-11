<template>
  <div class="big-container">
    <searchbox></searchbox>
    <div class="swiper">
      <swiper autoplay circular indicator-dots indicator-active-color="#fff">
        <swiper-item v-for="item in swiperList" :key="item.image_src">
          <img mode="aspectFill" :src="item.image_src">>
        </swiper-item>
      </swiper>
    </div>
    <!-- 分类按钮区域 -->
    <div class="category-container">
      <div class="item" v-for="item in categoryList">
        <img :src="item.image_src" alt>
        <p>{{item.name}}</p>
      </div>
    </div>
    <div class="floor-container">
      <div class="floor" v-for="item in floorList">
        <div class="floor-header">
          <img :src="item.floor_title.image_src" alt>
          <h3>{{item.floor_title.name}}</h3>
        </div>
        <div class="content">
          <img :src="item3.image_src" alt v-for="(item3, index3) in item.product_list">
        </div>
      </div>
    </div>
    <div class="bottom-line">
      <i class="iconfont icon-xiao"></i>
      我是有底线的
    </div>
    <!-- 返回顶部 -->
    <div class="back-top" @click="scrollTop" v-show="isShow">
      <i class="iconfont icon-jiantoushang"></i>
      顶部
    </div>
  </div>
</template>

<script>
import axios from "../../utils/index.js";
import searchbox from '../../components/searchbox.vue'
export default {
  data() {
    return {
      swiperList: [],
      categoryList: [],
      floorList: [],
      isShow: "true"
    };
  },

  components:{
    searchbox
  },

  methods: {
    scrollTop() {
      wx.pageScrollTo({
        scrollTop: 0,
        duration: 500
      });
      this.isShow = 'false';
    }
  },

  // 滚动事件
  onPageScroll(event) {
    // console.log(event);
    if(event.scrollTop>170){
      this.isShow=true;
    }else{
      this.isShow=false;
    }
  },

  // async created() {
  async onLoad() {
    // let swiperRes = await axios.get({
    //   url: "api/public/v1/home/swiperdata"
    // });
    // this.swiperList = swiperRes.data.message;

    // let categoryRes = await axios.get({
    //   url: "api/public/v1/home/catitems"
    // });
    // // console.log(categoryRes);
    // this.categoryList = categoryRes.data.message;

    // let floorRes = await axios.get({
    //   url: "api/public/v1/home/floordata"
    // });
    // // console.log(floorRes);
    // this.floorList = floorRes.data.message;

    let r1 = axios.get({
      url: "api/public/v1/home/swiperdata"
    });

    let r2 = axios.get({
      url: "api/public/v1/home/catitems"
    });

    let r3 = axios.get({
      url: "api/public/v1/home/floordata"
    });

    // Promise的all方法可以一次性发送所有请求，让页面一次性渲染完成
    let totalRes = await Promise.all([r1,r2,r3]);

    this.swiperList = totalRes[0].data.message;
    this.categoryList = totalRes[1].data.message;
    this.floorList = totalRes[2].data.message;
  }
};
</script>

<style lang='scss'>
$maincolor: #ff2d4a;
.big-container {
  padding-top: 100rpx;
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
  .swiper {
    width: 100%;
    img {
      width: 100%;
    }
  }

  // 分类按钮区域
  .category-container {
    display: flex;
    padding-top: 24rpx;
    padding-bottom: 29rpx;
    background-color: white;
    .item {
      flex: 1;
      img {
        display: block;
        width: 128rpx;
        height: 128rpx;
        margin: 0 auto;
      }
      p {
        text-align: center;
        font-size: 24rpx;
        color: #999999;
        font-family: "PingFang-SC-Medium";
        margin-top: 10rpx;
      }
    }
  }
  .floor-container {
    .floor {
      .floor-header {
        padding-top: 30rpx;
        height: 90rpx;
        box-sizing: border-box;
        position: relative;
        img {
          width: 100%;
          height: 100%;
          position: absolute;
          top: 0;
          left: 0;
        }
        h3 {
          color: #ff7b94;
          position: absolute;
          left: 30rpx;
          top: 50%;
          transform: translateY(-50%);
        }
      }
      .content {
        padding: 20rpx 0 0 16rpx;
        overflow: hidden;
        img {
          display: block;
          float: left;
          width: 230rpx;
          height: 190rpx;
          margin-right: 10rpx;
          &:first-child {
            height: 390rpx;
          }
          &:nth-child(2) {
            margin-bottom: 10rpx;
          }
          &:nth-child(3) {
            margin-bottom: 10rpx;
          }
        }
      }
    }
  }
  .bottom-line {
    display: flex;
    justify-content: center;
    color: #999999;
    font-size: 26rpx;
    margin-top: 20rpx;
    margin-bottom: 20rpx;
  }
  // 返回顶部
  .back-top {
    position: fixed;
    bottom: 15rpx;
    right: 15rpx;
    text-align: center;
    width: 90rpx;
    height: 90rpx;
    border-radius: 50%;
    background: rgba($color: #000000, $alpha: 0.5);
    font-size: 26rpx;
    color: #fff;
  }
}
</style>
