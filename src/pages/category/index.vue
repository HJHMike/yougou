<template>
  <div class="category-container">
    <searchbox></searchbox>
    <div class="scroll-box">
      <scroll-view scroll-y scroll-with-animation class="left">
        <ul>
          <li
            v-for="(item, i) in categoryList"
            :class="{actived:index === i}"
            :key="item.cat_id"
            @click="index=i"
          >{{item.cat_name}}</li>
        </ul>
      </scroll-view>

      <scroll-view scroll-y scroll-with-animation class="right">
        <img src="/static/titleImage.png" alt class="title-img">
        <block v-if="categoryList.length != 0">
          <div class="section" v-for="level1 in categoryList[index].children" :key="level1.cat_id">
            <div class="title">
              <span>/</span>
                &nbsp;&nbsp;&nbsp;&nbsp;{{level1.cat_name}}&nbsp;&nbsp;&nbsp;&nbsp;
              <span>/</span>
            </div>
            <div class="items">
              <div class="item" v-for="(level2,level2Index) in level1.children" :key="level2.cat_id">
                <img :src="'https://autumnfish.cn/wx/'+level2.cat_icon" alt>
                <p>{{level2.cat_name}}</p>
              </div>
            </div>
          </div>
        </block>
      </scroll-view>
    </div>
  </div>
</template>

<script>
import axios from "../../utils/index.js";
import searchbox from "../../components/searchbox";
export default {
  data() {
    return {
      categoryList: [],
      index: 0
    };
  },

  components: {
    searchbox
  },

  // 优化 ：让页面加载完再拿数据而不是整个程序一启动就拿
  // async created() {
  async onLoad() {
    // 获取分类数据
    let categoryRes = await axios.get({
      url: "api/public/v1/categories"
    });
    console.log(categoryRes);
    this.categoryList = categoryRes.data.message;
  }
};
</script>

<style lang='scss'>
$maincolor: #ff2d4a;
page {
  height: 100%;
}

.category-container {
  height: 100%;
  padding-top: 100rpx;
  box-sizing: border-box;
  .scroll-box {
    display: flex;
    height: 100%;
    scroll-view {
      height: 100%;
    }
    .left {
      width: 200rpx;
      li {
        font-size: 26rpx;
        text-align: center;
        height: 100rpx;
        line-height: 100rpx;
        background-color: #f4f4f4;
        &.actived {
          font-weight: 900;
          color: $maincolor;
          position: relative;
          background-color: #fff;
          &::before {
            content: "";
            position: absolute;
            width: 10rpx;
            height: 60rpx;
            background-color: $maincolor;
            left: 0;
            top: 20rpx;
          }
        }
      }
    }
    .right {
      flex: 1;
      padding: 15rpx;
      box-sizing: border-box;
      image {
        width: 100%;
        display: block;
        height: 200rpx;
      }
      .section {
        margin-top: 40rpx;
        .title {
          text-align: center;
          font-size: 28rpx;
          span {
            color: #ccc;
          }
        }
        .items {
          display: flex;
          flex-wrap: wrap;
          .item {
            margin-top: 40rpx;
            width: 33.33%;
            img {
              display: block;
              width: 100rpx;
              height: 70rpx;
              margin: 0 auto;
            }
            p {
              text-align: center;
              margin-top: 20rpx;
              font-size: 26rpx;
            }
          }
        }
      }
    }
  }
}

::-webkit-scrollbar {
  width: 0;
  height: 0;
  color: transparent;
}
</style>
