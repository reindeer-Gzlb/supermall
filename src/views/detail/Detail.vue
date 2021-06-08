<template>
  <div id="Detail">
    <detail-nav-bar class="detail-nav"  @titleClick = 'titleClick'/>
      <scroll class="detail-scroll" ref="scroll">
        <detail-swiper :top-images= "topImages" />
        <detail-base-info :goods= "goods"/>
        <detail-shop-info :shop= "shop"/>
        <detail-goods-info :detail-info = 'detailInfo'  @imageLoad = 'imageLoad' />
        <detail-param-info ref="params" :param-info= 'paramInfo'/>
        <detail-comment-info ref="comment" :comment-info="commentInfo" />
        <goods-list ref="recommend" :goods= "recommends" />
        
      </scroll>
      <detail-bottom-bar />
  </div>
</template>

<script>
import {getDetail,Goods,Shop,GoodsParam,getRecommend} from '@/network/detail.js'


import DetailNavBar from './chilComps/DetailNavBar.vue'
import DetailSwiper from './chilComps/DetailSwiper.vue'
import DetailBaseInfo from './chilComps/DetailBaseInfo.vue'
import DetailShopInfo from './chilComps/DetailShopInfo.vue'
import DetailGoodsInfo from './chilComps/DetailGoodsInfo.vue'
import DetailParamInfo from './chilComps/DetailParamInfo.vue'
import DetailCommentInfo from './chilComps/DetailCommentInfo.vue'
import DetailBottomBar from './chilComps/DetailBottomBar.vue'


import Scroll from '../../components/common/scroll/Scroll.vue'
import GoodsList from '../../components/common/goods/GoodsList.vue'
import {debounce} from '@/components/common/until/Until.js'


export default {
  name:'Detail',
  components:{DetailNavBar, DetailSwiper, DetailBaseInfo, DetailShopInfo, DetailGoodsInfo, DetailParamInfo,Scroll, DetailCommentInfo, GoodsList,DetailBottomBar},
  data(){ 
   return{
     iid:null,
     topImages:[],
     goods:{},
     shop:{},
     detailInfo:{},
     paramInfo:{},
     commentInfo:{},
     recommends:[],
     themTopYs:[],
     getThemTopY:null
   }
  },
  created(){
    // 保存传入的iid
    this.iid = this.$route.params.iid

    // 根据iid请求数据
    getDetail(this.iid).then(res =>{
      // console.log(res);
      const data = res.result
        this.topImages = res.result.itemInfo.topImages

        // 3获取商品信息
        this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)

        //  4店铺信息
        this.shop = new Shop(data.shopInfo)
        
        // 5保存商品的详细信息
        this.detailInfo = data.detailInfo

        // 6获取参数信息
        this.paramInfo = new GoodsParam(data.itemParams.info,data.itemParams.rule)

        // 7评论信息
        if(data.rate.cRate !== 0)
        this.commentInfo = data.rate.list[0]

      
    })


    getRecommend().then(res => {


      this.$nextTick(() =>{
        this.themTopYs = []
          this.themTopYs.push(0);
          this.themTopYs.push(this.$refs.params.$el.offsetTop)
          this.themTopYs.push(this.$refs.comment.$el.offsetTop)
          this.themTopYs.push(this.$refs.recommend.$el.offsetTop)
          
          console.log(this.themTopYs);
      })
      this.recommends = res.data.list
    })

    this.getThemTopY = debounce(() => {
      this.themTopYs = []
          this.themTopYs.push(0);
          this.themTopYs.push(this.$refs.params.$el.offsetTop)
          this.themTopYs.push(this.$refs.comment.$el.offsetTop)
          this.themTopYs.push(this.$refs.recommend.$el.offsetTop)
          
          console.log(this.themTopYs);
    },100)

  },
  mounted(){
  },
  updated(){
  },
  methods:{
    imageLoad(){
      this.$refs.scroll.refresh()
      this.getThemTopY()
    },
    titleClick(index){
      this.$refs.scroll.scrollTo(0, -this.themTopYs[index],200)
    }
  }
}
</script>

<style>
#Detail{
  position: relative;
  z-index: 10;
  background-color: #fff;
  height: 100vh;
}
.detail-scroll{
  height: calc(100% - 44px);
}
.detail-nav{
  position: relative;
  background-color: #fff;
  z-index: 10;

}
</style>