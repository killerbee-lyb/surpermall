<template>
  <swiper>
    <swiper-item v-for="item in banners" :key='item.link'>
      <a :href="item.link">
        <img :src="item.image" @load="imgload">
      </a>
    </swiper-item>
  </swiper>
</template>

<script>
  import {
    Swiper,
    SwiperItem
  } from '../../../components/common/swiper'

  export default {
    name: "HomeSwiper",
    props: {
      banners: {
        type: Array,
        default () {
          return []
        }
      }
    },
    components: {
      Swiper,
      SwiperItem
    },
    methods: {
      imgload() {
       if(!this.isload){
         this.$emit('swiperImageLoad')
         this.isload = true
         }
      }
    }

  }
</script>

<style>
</style>
