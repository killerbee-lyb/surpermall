<template>
  <div>
    <nav-bar>
      <div slot="left" class="back" @click="backclick">
      <img src="../../../assets/img/common/back.svg" alt="">
      </div>
      <div slot="center" class="title">
        <div class="title-item" v-for="(item,index) in titles" :key="item.index"
         :class="{active:index===currentindex}" @click="titleclick(index)">
          {{item}}</div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from '../../../components/common/navbar/NavBar'
  export default {
    name: "DetailNavBar",
    components: {
      NavBar
    },
    data() {
      return {
        titles: ['商品', '参数', '评论', '推荐'],
        currentindex: 0
      }
    },
    methods:{
      titleclick(index){
        this.currentindex = index
      },
      backclick(){
        this.$router.go(-1)
      }
    }
  }
</script>

<style>
  .title {
    display: flex;
  }

  .title-item {
    flex: 1;
  }

  .active {
    color: var(--color-high-text);
  }
  .back img{
    margin-top: 10px;
  }
</style>
