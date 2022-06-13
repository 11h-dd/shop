<template>
  <view>
    <!-- //轮播图部分 -->
    <swiper class="swiperList" :indicator-dots="true" :autoplay="true" :interval="1500" :duration="1000" circular
      indicator-active-color="#fff">
      <swiper-item v-for="(item,index) in swiperList" :key="index">
        <navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
          <image :src="item.image_src" mode=""></image>
        </navigator>
      </swiper-item>
    </swiper>
    <!-- //分类导航区域 -->
    <view class="navList-item">
        <view class="nav-item" v-for="(item,index) in navList" :key="index" @click="navClickHandler(item)">
          <image :src="item.image_src" mode="" class="nav-img"></image>
          
        </view>
    </view>
  </view>
</template>

<script>
  import {
    swiperAPI,
    cateAPI
  } from '@/Api'
  // import axios from '@/rquest/index.js'
  export default {
    data() {
      return {
        swiperList: [],
        navList:[]
      };
    },
    onLoad() {
      this.getSwiperList()
     this.getNavList()
    },
    methods: {
      navClickHandler(item){
           if (item.name === '分类') {
              uni.switchTab({
                url: '/pages/cate/cate'
              })
            }
          
      },
      async getSwiperList() {
        let {
          data: res
        } = await swiperAPI('/api/public/v1/home/swiperdata')
          if (res.meta.status !== 200) return uni.$showMsg()
        this.swiperList = res.message
      },
      async getNavList(){
          const { data : res } = await cateAPI('/api/public/v1/home/catitems')
          if(res.meta.status !== 200) return uni.$showMsg()
          this.navList = res.message
          console.log(this.navList,'navList')
      },
    },

  
  }
</script>

<style lang="scss">
  .swiperList {
    height: 330rpx;

    .swiper-item,
    image {
      width: 100%;
      height: 100%;
    }
  }
  .navList-item {
    display: flex;
    justify-content: space-around;
    margin: 15px 0;
    .nav-img {
      width: 128rpx;
      height: 140rpx;
    }
  }
</style>
