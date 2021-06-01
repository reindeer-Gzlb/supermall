<template>
   <tab-bar> 
     <tab-bar-item  path="/home" activeColor='pink'>
       <img slot="item-icon" src="../assets/img/tabbar/shouye.svg" alt="">
       <img slot='item-icon2' src="../assets/img/tabbar/shouye2.svg" alt="">
      <div slot="item-text">首页</div>
     </tab-bar-item> 

     <tab-bar-item path="/category" activeColor='pink'>
       <img slot="item-icon" src="../assets/img/tabbar/fenlei.svg" alt="">
       <img slot='item-icon2' src="../assets/img/tabbar/fenlei2.svg" alt="">
       <div slot="item-text">分类</div>
     </tab-bar-item>

     <tab-bar-item path="/cart" activeColor='pink'>
       <img slot="item-icon" src="../assets/img/tabbar/gouwuche.svg" alt="">
         <img slot="item-icon2" src="../assets/img/tabbar/gouwuche2.svg" alt="">
         <div slot="item-text">购物车</div>
     </tab-bar-item>

     <tab-bar-item path="/profile" activeColor='pink'>
       <img slot="item-icon" src="../assets/img/tabbar/mine.svg" alt="">
       <img slot="item-icon2" src="../assets/img/tabbar/mine2.svg" alt="">
      <div slot="item-text">我的</div>
     </tab-bar-item>

   </tab-bar>
</template>

<script>
import TabBar from './tabbar/TabBar'
import TabBarItem from './tabbar/TabBarItem'

export default {
  name:'MainTabBar',
  components:{
    TabBar,
    TabBarItem
  }
}
</script>

<style>

</style>