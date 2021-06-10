<template>
  <div class="cart">
    <nav-bar class="cart-nav-bar">
      <div slot="center">购物车({{length}})</div>
    </nav-bar>
    <cart-list></cart-list>
    <bottom-bar></bottom-bar>
  </div>
</template>

<script>
import NavBar from '../../components/common/navbar/NavBar.vue'
import CartList from './childcomps/CartList.vue'
import BottomBar from './childcomps/BottomBar.vue'

import {mapGetters} from  'vuex'

export default {
  components: { NavBar, CartList,BottomBar },
  name:'Cart',
  computed:{
    // ...mapGetters(['cartLength','cartList'])
    ...mapGetters({
      length:'cartLength',
      list:'cartList'
    })
  }
}
</script>

<style>
.cart{
  height: 100vh;
}
.cart-nav-bar{
  background-color: var(--color-tint);
  color: #fff;
  font-weight: 700;
}
</style>