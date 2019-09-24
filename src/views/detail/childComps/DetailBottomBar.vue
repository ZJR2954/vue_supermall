<template>
    <div class="bottom-bar">
      <div class="bar-item bar-left">
        <div>
          <img class="icon" src="~assets/img/detail/kefu.png">
          <span class="text">客服</span>
        </div>
        <div>
          <img class="icon" src="~assets/img/detail/dianpu.png">
          <span class="text">店铺</span>
        </div>
        <div>
          <img class="icon" src="~assets/img/detail/shoucang.png">
          <span class="text">收藏</span>
        </div>
      </div>
      <div class="bar-item bar-right">
        <div class="cart" @click="addToCart">加入购物车</div>
        <span class="buy">购买</span>
      </div>
    </div>
</template>

<script>
  export default {
    name: "DetailBottomBar",
    methods: {
      addToCart() {
        this.$emit('addCart')
      }
    }
  }
</script>

<style scoped>
  .bottom-bar {
    height: 58px;
    background-color: #fff;
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    display: flex;
    text-align: center;
  }
  .bar-item {
    flex: 1;
    display: flex;
  }

  .bar-item>div {
    flex: 1;
  }

  .bar-item .text {
    font-size: 13px;
  }

  .bar-left .icon {
    display: block;
    width: 22px;
    height: 22px;
    margin: 10px auto 3px;

  }

  .bar-left .service {
    background-position: 0 -54px;
  }

  .bar-left .shop {
    background-position: 0 -98px;
  }

  .bar-right {
    display: flex;
    font-size: 15px;
    color: #fff;
    line-height: 58px;
  }

  .bar-right .cart {
    flex: 1;
    background-color: #ffe817;
    color: #333;
  }

  .bar-right .buy {
    flex: 1;
    background-color: #f69;
  }

</style>
