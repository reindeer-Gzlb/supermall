<template>
  <div>
    <nav-bar>
      <div slot="left" class="back" @click="backClcik">
        <img src="@/assets/img/common/back.svg" alt="">
      </div>
      <div slot="center" class="title11" >
        <div v-for="(item,index) in titles" :key="index" 
        class="title-item11" 
        @click="itemClick(index)"
        :class="{active11: index === currentIndex}">
          {{item}}
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
import NavBar from '../../../components/common/navbar/NavBar.vue'

export default {
  components: { NavBar },
  name:'DetailNavBar',
  data(){
    return{
      titles:['商品','参数','评论','推荐'],
      currentIndex:0
    }
  },
  methods:{
    itemClick(index){
      this.currentIndex = index
      this.$emit('titleClick',index)
    },
    backClcik(){
      this.$router.back()
    }
  }
}
</script>

<style>
  .title11{
    display: flex;
    font-size: 14px;
  }
  .title-item11{
    flex: 1;
  }
  .active11{
    color:salmon;
  }
  .back{
    margin-top: 6px;
  }
</style>