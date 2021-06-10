<template>
  <div class="cart-list">
      <scroll class="content1">
        <cart-list-item v-for="(item,index) in cartList" :key="index" :item-info='item'></cart-list-item>
      </scroll>
  </div>
</template>

<script>
import {mapGetters} from 'vuex'
import Scroll from '../../../components/common/scroll/Scroll.vue'
import CartListItem from './CartListItem.vue'

export default {
  components: { Scroll, CartListItem },
  name:'CartList',
  computed:{
    ...mapGetters(['cartList'])
  }
}
</script>

<style>
 .cart-list{
   height:100vh;
 }
.content1{
  height:calc(100vh - 44px - 49px - 40px);
  overflow: hidden;
}
</style>