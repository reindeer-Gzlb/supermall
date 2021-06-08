<template>
  <div class="recommend">
    <div class="recommend-item" v-for="(item,index) in recommends" :key="index">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name:'RecommendViews',
  props:{
    recommends:{
      type:Array,
      default(){
        return []
      }
    }
  }
}
</script>

<style>
 .recommend{
   display: flex;
   padding-top: 8px;
   border-bottom: 8px solid #eee;
   padding-bottom: 10px;
   width: 100%;
 }
  .recommend-item{
    flex: 1;
    text-align-last: center;

  }
  .recommend-item img{
    width: 75px;
    height: 75px;
    margin-bottom: 10px;
  }
</style>