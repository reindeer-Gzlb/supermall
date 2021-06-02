<template>
  <div class="tab-bar-item" @click="ItemClick">
   
    <slot  v-if="!flag" name="item-icon"></slot>
    <slot  v-else name="item-icon2"></slot>
    <div :style="classcolor"><slot name="item-text"></slot></div>
    
  </div>
</template>

<script>
export default { 
  name:'TabBarItem',
  props :{
    path:String,
    activeColor:{
      type:String,
      default:'red'
    }
  },
  data(){
    return{
      // flag:true
    }
  },
  computed:{
    flag(){
      return this.$route.path.indexOf(this.path) !== -1
    },
    classcolor(){
      return   this.flag?{color: this.activeColor} : {}
    },
  },
  methods:{
    ItemClick(){
      this.$router.replace(this.path).catch(err => err);
    }
  }
}
</script>

<style>
  
.tab-bar-item{
  flex: 1;
  text-align: center;
  height: 49px;
  font-size: 14px;
}

.tab-bar-item img{
  width: 24px;
  height: 24px;
  margin-top: 3px;
  margin-bottom: 2px;
  /* 去掉图片下面的多余像素 */
  vertical-align: middle;
}


</style>