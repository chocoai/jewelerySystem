<template>
  <div class="home-container">
    <img src="cloud://test-c9f00f.7465-test-c9f00f/jewelry/top-bgc.png"
         mode='aspectFill'
         class="top-bgc">
    <span class="shop-title">店铺列表</span>
    <div class="have-shop"
         v-if="shopsList.length > 0">
      <Shop :shop='shop'
            :index='i'
            v-for="(shop,i) in shopsList"
            :key="i"
            @goDel="goDel"></Shop>
    </div>
    <div class="no-shop"
         v-else>
      <div class="begin-shop">
        <img src="cloud://test-c9f00f.7465-test-c9f00f/jewelry/no-shops.png"
             mode='aspectFill'
             class="no-shop-alt">
        <span class="no-shop-words">暂无店铺诊断记录</span>
        <!-- <button class="begin">开始诊断</button> -->
      </div>
    </div>
  </div>
</template>
<script>
import api from '../../../config/api.js'
import Shop from './shop'
import { mapState } from "vuex";
import wxUtils from '../../utils/wxUtils';

export default {
  components: {
    Shop
  },
  data() {
    return {
      shopsList: [],
    }
  },
  methods: {
    goDel(shopId, index) {
      wxUtils.request(api.ShopDelete, this, { shopIds: shopId })
        .then(res => {
          wx.showToast({
            title: '删除成功',
            duration: 1500,
            mask: false,
          });
          this.shopsList.splice(index, 1)
        })
    },
  },
  async onShow() {
    let { data } = await wxUtils.request(api.ShopList, this)
    this.shopsList = data.shopsList
  }
}
</script>
<style lang="less" scoped>
.home-container {
  display: flex;
  flex-direction: column;
  .top-bgc {
    width: 100%;
    height: 296rpx;
  }
  .shop-title {
    color: #3a3a3a;
    font-size: 38rpx;
    font-weight: bold;
    margin-left: 30rpx;
    margin-top: 42rpx;
    margin-bottom: 40rpx;
  }
  .no-shop {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    .begin-shop {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      .no-shop-alt {
        width: 264rpx;
        height: 274rpx;
        margin-bottom: 10rpx;
      }
      .no-shop-words {
        font-size: 26rpx;
        color: #9b9b9b;
      }
      .begin {
        width: 296rpx;
        height: 76rpx;
        border-radius: 42rpx;
        background-color: #c1a46c;
        color: #fff;
        text-align: center;
        line-height: 76rpx;
        font-size: 30rpx;
        margin-top: 48rpx;
        font-weight: bold;
      }
    }
  }
  .have-shop {
    width: 100%;
    overflow: hidden;
    padding-left: 30rpx;
  }
}
</style>
