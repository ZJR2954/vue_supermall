<template>
    <div class="cart-list">
      <scroll class="content" ref="scroll">
        <!--<li v-for="item in $store.state.cartList">{{item}}</li>-->
        <cart-list-item v-for="(item,index) in $store.state.cartList"
                        :key="index"
                        :product="item"/>
        <!--<li v-for="item in 100">{{item}}</li>-->
      </scroll>
    </div>
</template>

<script>
  import Scroll from 'components/common/scroll/Scroll'
  import CartListItem from './CartListItem'

  export default {
    name: "CartList",
    components: {
      Scroll,
      CartListItem
    },
    activated() {
      this.$refs.scroll.refresh()
    }
  }
</script>

<style scoped>
  .cart-list {
    height: 100vh;
  }
  .content {
    height: calc(100% - 44px - 49px - 40px);
    overflow: hidden;
  }
</style>
