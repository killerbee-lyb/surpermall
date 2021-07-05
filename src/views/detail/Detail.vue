<template>
  <div id="detail">
    <detail-nav-bar></detail-nav-bar>
    <detail-swiper :top-image="topImages"></detail-swiper>
    <detail-base-info :goods="goods"> </detail-base-info>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import DetailBaseInfo from './childComps/DetailBaseInfo'
  import {getDetail,Goods} from '../../network/detail'
  export default{
    name:"Detail",
    data(){
      return{
        iid:null,
        topImages:[],
        goods:{}
      }
    },
    components:{
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo
    },
    created() {
      this.iid = this.$route.params.iid
      getDetail(this.iid).then(res => {
        console.log(res)
        const data = res.result
        this.topImages= res.result.itemInfo.topImages
        this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)
      })
    }
  }
</script>

<style>
</style>
