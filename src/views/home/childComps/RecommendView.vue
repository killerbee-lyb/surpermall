<template>
  <div class="recommend">
    <div v-for="item in recommends" :key="item.link " class="recommend-item">
        <a :href="item.link">
          <img :src="item.image" />
          <div>{{item.title}}</div>
        </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: "RecommendView",
    props:{
      recommends:{
        type:Array,
        default() {
          return[]
        }
      }
    },
    }
</script>

<style>
  .recommend{
    display: flex;
    width: 100%;
    text-align: center;
    font-size: 12px;
    padding: 15px 0 ;
    border-bottom:8px solid #d4d4d4;
  }
  .recommend-item{
    flex: 1;
  }
  .recommend-item img{
    width: 65px;
    height: 65px;
    margin-bottom: 10px;
  }
</style>
