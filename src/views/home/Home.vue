<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
     <tab-control class="tab-control" :title="['流行','新款','精选']" 
                 @tabClick="tabClick"
                 ref="tabControl1"
                 v-show="isTabFixed">
     </tab-control>
    <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentscroll" :pull-up-load='true' @pullingUp='loadMore'>
    
       <home-swiper :banners='banners' @swiperImageLoad='swiperImageLoad' class="top"></home-swiper>
    <recommend-views :recommends="recommends"></recommend-views>
    <feature-view></feature-view>
    <tab-control :title="['流行','新款','精选']" 
                 @tabClick="tabClick"
                 ref="tabControl2">
     </tab-control>
    <goods-list :goods="showType"></goods-list>
    </scroll>
    <back-top @click.native='backClick' v-show="isShowBackTop"></back-top>

  </div>
</template>
 
<script>

import HomeSwiper from './childComps/HomeSwiper.vue'
import RecommendViews from './childComps/RecommendViews.vue'
import FeatureView from './childComps/FeatureView.vue'


import NavBar from '../../components/common/navbar/NavBar.vue'
import TabControl from '../../components/common/tabControl/TabControl.vue'
import GoodsList from '../../components/common/goods/GoodsList.vue'
import Scroll from '../../components/common/scroll/Scroll'   
import BackTop from '../../components/common/backtop/BackTop.vue'



import {getHomeMultidata , getHomeGoods} from '../../network/Home.js'
import {debounce} from '@/components/common/until/Until'









export default {
  components: { NavBar, HomeSwiper, RecommendViews, FeatureView, TabControl, GoodsList, Scroll, BackTop,},
  name:'Home',
  data(){
    return{
      banners:[],
      recommends:[],
      goods:{
        'pop': { page:0 , list:[]},
        'new': { page:0 , list:[]},
        'sell': { page:0 , list:[]}
      }, 
      currentType: 'pop',
      isShowBackTop:'flase',
      tabtop:0,
      isTabFixed:false,
      saveY:0
    }
  },
  created(){
    this.getHomeMultidata()
// 请求商品数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },

  mounted(){
    // 监听item图片的加载
  const refresh = debounce( this.$refs.scroll.refresh,200)
  
  this.$bus.$on('itemImageLoad',() => {
      refresh()
    })
  },


  computed:{
    showType(){
      return this.goods[this.currentType].list
    }
  },
    activated(){ 
    this.$refs.scroll.refresh()
    this.$refs.scroll.scrollTo(0,this.saveY,0)
  },
  deactivated(){
   this.saveY  = this.$refs.scroll.saveY()
  },
  methods:{
    //  监听事件点击方法
    tabClick(index){
        switch(index){
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
        }
        this.$refs.tabControl1.counter = index;
        this.$refs.tabControl2.counter = index;
    },
    backClick(){
      this.$refs.scroll.scrollTo(0,0,500)
      },
    contentscroll(position){
      // 判断backTop是否显示
      this.isShowBackTop = -(position.y)  > 1000
      // 判断tabControl是否吸顶
      this.isTabFixed = -(position.y) > this.tabtop -40
    },
    loadMore(){
      this.getHomeGoods(this.currentType)},
    swiperImageLoad(){
      this.tabtop = this.$refs.tabControl2.$el.offsetTop
    },






    //  网络请求方法
    getHomeMultidata(){
      getHomeMultidata().then( res => {
      this.banners = res.data.banner.list;
      this.recommends = res.data.recommend .list;
    })
    },
    getHomeGoods(type){
      const page = this.goods[type].page + 1
      getHomeGoods(type,page).then( res => {
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1

        //完成上拉加载更多
        this.$refs.scroll.finishPullUp()
    }) 
    }
  }
 
}
</script>

<style scoped>
#home{
  height: 100vh;
}
  .home-nav{
    position: relative;
    background-color: var(--color-tint);
    color: white; 
    z-index: 100;
  }

  .content{
    position: absolute;
    top: 0px;
    bottom: 49px;
  }
  .tab-control{
    position: relative;
    z-index: 9;
  }
  .top{
    padding-top: 40px;
  }
</style>