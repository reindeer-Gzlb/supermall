<template>
  <div  ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'

export default {
  name:'Scroll',
  props:{
    probeType:{
      type:Number,
      default:0
    },
  },
  data(){
    return{
      scroll:{},
      pullUpLoad:{
        type:Boolean,
        default:false
      }
    }
  },

  mounted(){
    // 创建scroll对象
    this.scroll = new BScroll(this.$refs.wrapper,{
      click:true,
      probeType:this.probeType,
      pullUpLoad:this.pullUpLoad
    })
    // 监听滚动位置
   if(this.probeType ===2 || this.probeType === 3){
      this.scroll.on('scroll',(position)=>{
      this.$emit('scroll',position)
    })
   }

    if(this.pullUpLoad){
      this.scroll.on('pullingUp',()=>{
        this.$emit('pullingUp') 
      })
    }
  },

  methods:{
    scrollTo(x,y,time=300){
      this.scroll.scrollTo(x,y,time)
    },
    finishPullUp(){
      this.scroll.finishPullUp()
    },
    refresh(){
      // 先判断scroll是否有值，防止在home。vue里加载scroll。refresh时，scroll还没加载完成
      this.scroll && this.scroll.refresh()
    },
    finishPullUp(){
      this.scroll && this.scroll.finishPullUp()
    },
    saveY(){
      return this.scroll ? this.scroll.y : 0
    }
  }
  
}
</script>

<style>

</style>