<template>
  
    <swiper class="topimg">
      <swiper-item v-for="item in topImage" :key="item.index">
        <img :src="item" alt="">
      </swiper-item>
    </swiper>
  
</template>

<script>
  import {Swiper,SwiperItem} from '../../../components/common/swiper'
  export default{
    name:"DetailSwiper",
    components:{
      Swiper,
      SwiperItem
    },
    props:{
      topImage:{
        type:Array,
        default(){
          return[]
        }
      }
    }
  }
</script>

<style>
  .topimg{
    height: 300px;
    overflow: hidden;
  }
</style>
