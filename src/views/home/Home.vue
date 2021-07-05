<template>
  <div id="home" class="wrapper">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control :titles="['流行','新款','精选']"  @tabClick="tabClick" ref="tabControl1"
     class="tab-control" v-show="isTabFixed"
    ></tab-control>
    <scroll class="content" ref="scroll"
     :probe-type="3" @scroll="contentScroll"
     :pull-up-load="true" @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control :titles="['流行','新款','精选']"  @tabClick="tabClick" ref="tabControl2"
       v-show="!isTabFixed"
      ></tab-control>
      <goods-list :goods="goods[currentType].list"></goods-list>
    </scroll>
    <back-top @click.native="backclick" v-show="IsShowBackTop"></back-top>

  </div>
</template>

<script>
  import NavBar from '../../components/common/navbar/NavBar'
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'
  import TabControl from '../../components/content/tabControl/TabControl'
  import GoodsList from '../../components/content/goods/GoodsList'
  import BackTop from '../../components/content/backTop/BackTop'
  import Scroll from '../../components/common/scroll/Scroll'
  import {
    gethomemultidata,
    gethomegoods
  } from "../../network/home"

  export default {
    name: "Home",
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {
            page: 0,
            list: []
          },
          'new': {
            page: 0,
            list: []
          },
          'sell': {
            page: 0,
            list: []
          },
        },
        currentType: 'pop',
        IsShowBackTop:false,
        tabOffsetTop:0,
        isTabFixed:false
      }
    },
    created() {
      this.gethomemultidata()
      this.gethomegoods('pop')
      this.gethomegoods('new')
      this.gethomegoods('sell')
    },
    methods: {
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
        this.$refs.tabControl1.currentIndex = index
         this.$refs.tabControl2.currentIndex = index

      },
      backclick(){

        this.$refs.scroll.scrollTo(0,0)
      },
      contentScroll(position){
        // console.log(this.IsShowBackTop)
        this.IsShowBackTop = (-position.y) > 1000
        this.isTabFixed =  (-position.y) > this.tabOffsetTop
      },
      loadMore(){
       this.gethomegoods(this.currentType)
       this.$refs.scroll.scroll.refresh()
      },
      gethomemultidata() {
        gethomemultidata().then(res => {
          console.log(res);
          // this.result = res;
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      gethomegoods(type) {
        const page = this.goods[type].page + 1
        gethomegoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
          this.$refs.scroll.finishPullUp()
        })
      },
      swiperImageLoad(){
        this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop

      }


    },
    mounted() {
      this.tabOffsetTop = this.$refs.tabControl
    }
  }
</script>

<style scoped>
  #home {
    /* padding-top: 44px; */
    height: 100vh;
    /* position: relative; */
  }

  .home-nav {
    background-color: var(--color-tint);
    color: white;
   /* position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9; */
  }

  /* .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  } */

  .content {
     height: calc(100% - 49px);
    overflow: hidden;
  }
 /* .fixed{
    position: fixed;
    left: 0;
    right: 0;
    top: 44px;
  } */
  .tab-control{
    position: relative;
    z-index: 9;

  }
</style>
