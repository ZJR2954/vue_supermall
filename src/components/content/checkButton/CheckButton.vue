<template>
    <div class="check-button">
      <img v-show="!isChecked" src="~assets/img/cart/xuanze1.png">
      <img v-show="isChecked" src="~assets/img/cart/xuanze2.png">
    </div>
</template>

<script>
  export default {
    name: "CheckButton",
    props: {
      isChecked: {
        type: Boolean,
        default() {
          return false
        }
      }
    }
  }
</script>

<style scoped>
  .check-button img {
    width: 20px;
    height: 20px;
  }
</style>
