<template>
    <div class="bottom-bar">
      <div class="check-content">
        <check-button class="check-button"
                      :is-checked="isSelectAll"
                      @click.native="checkClick"/>
        <span><div>全选</div></span>
      </div>
      <div class="price">
        合计：{{totalPrice}}
      </div>
      <div class="calculate" @click="calcClick">
        去结算:({{checkLength}})
      </div>
    </div>
</template>

<script>
  import CheckButton from 'components/content/checkButton/CheckButton'
  export default {
    name: "CartBottomBar",
    components: {
      CheckButton
    },
    methods: {
      checkClick() {
        if(this.isSelectAll){
          this.$store.state.cartList.forEach(item => item.checked = false)
        }else {
          this.$store.state.cartList.forEach(item => item.checked = true)
        }

        // this.$store.state.cartList.forEach(item => item.checked = !this.isSelectAll)
      },
      calcClick() {
        if (!this.isSelectAll) {
          this.$toast.show('请选择购买的商品', 2000)
        }
      }
    },
    computed: {
      totalPrice() {
        return this.$store.state.cartList.filter(item => {
          return item.checked
        }).reduce((preValue, item) => {
          return preValue + item.price * item.count
        }, 0).toFixed(2)//保留两位小数
      },
      checkLength() {
        return this.$store.state.cartList.filter(item => {
          return item.checked
        }).length
      },
      isSelectAll() {
        // return !this.$store.state.cartList.filter(item => !item.checked).length
        if(this.$store.state.cartList.length === 0) return false
        return !this.$store.state.cartList.find(item => !item.checked)
      }
    }
  }
</script>

<style scoped>
  .bottom-bar {
    position: absolute;
    bottom: 49px;
    z-index: 9;
    width: 100%;
    height: 40px;
    line-height: 40px;
    background-color: #eee;

    display: flex;
  }
  .check-content {
    width: 60px;
    display: flex;
    align-items: center;
    margin-left: 10px;

  }
  .check-button {
    width: 22px;
    height: 22px;
    line-height: 22px;
    margin-right: 5px;
  }
  .price {
    flex: 1;
    margin-left: 20px;
  }
  .calculate {
    width: 90px;
    background-color: orange;
    text-align: center;
  }
</style>
