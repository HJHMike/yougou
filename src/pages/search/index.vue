<template>
  <div class="search-container">
    <!-- 顶部搜索盒子 -->
    <div class="top">
      <icon type="search" size="16"></icon>
      <input type="text" placeholder="请输入你想要的商品" v-model.trim="searchData" @change="search">
      <button>取消</button>
    </div>
    <!-- 底部历史记录或者商品盒子 -->
    <dic class="content">
      <!-- 历史记录盒子 -->
      <div class="history" v-show="resultList.length==0">
        <div class="title">
          <span>历史搜索</span>
          <i class="iconfont icon-shanchu" @click="delAll"></i>
        </div>
        <div class="items">
          <div class="item" v-for="item in historyList" @click="fillandSearch(item)">
            {{item}}
            <i @click="delOne(index)" class="iconfont icon-shanchu"></i>
          </div>
        </div>
      </div>
      <!-- 商品盒子 -->
      <div class="result" v-show="resultList.length!=0">
        <div class="filter">
          <div class="active">综合</div>
          <div>销量</div>
          <div>价格
            <div class="arrow">
              <span>▲</span>
              <span>▼</span>
            </div>
          </div>
        </div>
        <div class="goods">
          <div class="item" v-for="(item, index) in resultList">
            <div class="left">
              <img :src="item.goods_small_logo" alt="">
            </div>
            <div class="right">
              <div class="goods_name">
                {{item.goods_name}}
              </div>
              <div class="goods_price">
                ￥<span>{{item.goods_price}}</span>.00
              </div>
            </div>
          </div>
        </div>
      </div>
    </dic>
  </div>
</template>

<script>
/*
  点击回车 搜索  数据常驻  往列表中添加历史记录  重复去掉放前面  点击x清除历史列表
*/

import axios from "../../utils/index.js";

export default {
  data() {
    return {
      // 搜索数据
      searchData: "",
      // 历史记录列表
      historyList: [],
      // 商品列表
      resultList: []
    };
  },

  methods: {
    async search() {
      // 清除重复项
      let index = this.historyList.indexOf(this.searchData);
      if (index != -1) {
        this.historyList.splice(index, 1);
      }

      // 长度限制
      if (this.historyList.length >= 10) {
        // this.searchList.splice(0,1);
        this.historyList.shift();
      }

      // 非空判断
      if (this.searchData === "") {
        return;
      }

      this.historyList.push(this.searchData);
      // this.searchData = '';

      let res = await axios.get({
        url: `api/public/v1/goods/search?query=${this.searchData}`
      });
      console.log(res);
      this.resultList = res.data.message.goods;
    },
    delOne(index) {
      this.historyList.splice(index, 1);
    },

    // 删除所有历史
    delAll() {
      wx.showModal({
        title: "提示", //提示的标题,
        content: "是否删除所有历史记录", //提示的内容,
        showCancel: true, //是否显示取消按钮,
        cancelText: "取消", //取消按钮的文字，默认为取消，最多 4 个字符,
        cancelColor: "#000000", //取消按钮的文字颜色,
        confirmText: "确定", //确定按钮的文字，默认为取消，最多 4 个字符,
        confirmColor: "#3CC51F", //确定按钮的文字颜色,
        success: res => {
          if (res.confirm) {
            // console.log('用户点击确定')
            this.historyList = [];
          } else if (res.cancel) {
            // console.log('用户点击取消')
          }
        }
      });
    },

    // 点击标签填充输入框并且搜索
    fillandSearch(item) {
      this.searchData = item;
      this.search();
    }
  },

  // 页面创建读取历史
  created() {
    let res = wx.getStorageSync("yougouHistory");
    if (res) {
      this.historyList = res;
    }
  },

  // 利用侦听器设置wx的数据常驻
  watch: {
    historyList() {
      wx.setStorage({
        key: "yougouHistory",
        data: this.historyList
      });
    }
  }
};
</script>

<style lang='scss'>


page {
  height: 100%;
}

.search-container {
  .top {
    background-color: #eeeeee;
    padding: 30rpx 15rpx;
    position: relative;
    display: flex;
    icon {
      position: absolute;
      left: 35rpx;
      top: 50%;
      transform: translateY(-50%);
    }
    input {
      flex: 1;
      height: 60rpx;
      padding-left: 70rpx;
      margin-right: 25rpx;
      font-size: 25rpx;
      color: #aaa;
      background: white;
    }
    button {
      font-size: 28rpx;
      text-align: center;
      line-height: 60rpx;
      width: 160rpx;
      height: 60rpx;
    }
  }
  .content {
    .history {
      padding: 30rpx 15rpx;
      .title {
        height: 30rpx;
        line-height: 30rpx;
        display: flex;
        justify-content: space-between;
        span {
          font-size: 30rpx;
        }
        i {
          color: #cccccc;
        }
      }
      .items {
        margin-top: 30rpx;
        flex-wrap: wrap;
        .item {
          height: 65rpx;
          line-height: 65rpx;
          display: inline-block;
          padding: 0 25rpx;
          background-color: #dddddd;
          margin-right: 30rpx;
          margin-bottom: 20rpx;
          font-size: 24rpx;
          position: relative;
          border-radius: 65rpx;
          i {
            color: #999;
            position: absolute;
            top: 0;
            right: 0;
            transform: translate(50%, -50%);
          }
        }
      }
    }
    .result {
      padding-top: 100rpx;
      .filter {
        height: 100rpx;
        border-bottom: 1rpx #ddd solid;
        align-items: center;
        display: flex;
        > div {
          flex: 1;
          text-align: center;
          font-size: 30rpx;
          &.active{
            color: #ff2d4a;
          }
          &:last-child {
            display: flex;
            align-items: center;
            justify-content: center;
            .arrow {
              padding: 100rpx 10rpx;
              span {
                display: block;
                transform: scaleY(.6);
              }
            }
          }
        }
      }
      .goods{
        padding-left: 20rpx;
        .item{
          height: 260rpx;
          display: flex;
          border-bottom: 1rpx solid #ddd;
          padding: 30rpx 0;
          box-sizing: border-box;
          .left{
            margin-right: 20rpx;
            img{
              display: block;
              width: 200rpx;
              height: 200rpx;
            }
          }
          .right{
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding-right: 35rpx;
            .goods_name{
              background-color: #fff;
              font-size: 30rpx;
              overflow: hidden;
              text-overflow: ellipsis;
              display: -webkit-box;
              -webkit-box-orient: vertical;
              -webkit-line-clamp: 2;
            }
            .goods_price{
              font-size: 24rpx;
              color: #ff2d4a;
              span{
                font-size: 36rpx;
              }
            }
          }
        }
      }
    }
  }
}

.search-container{
  position: relative;
  padding-top: 120rpx;
  .top{
    position: fixed;
    width: 100%;
    left: 0;
    top: 0;
    z-index: 999;
  }
  .content{
    position: relative;
    padding-top: 100rpx;
    .filter{
      position: fixed;
      width: 100%;
      left: 0;
      top: 120rpx;
      background-color: #fff;
    }
  }
}
</style>
