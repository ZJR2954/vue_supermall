<template>
    <div v-if="Object.keys(goods).length!=0">
      <div class="info-title">{{goods.title}}</div>
      <div class="info-price">
        <span class="n-price">{{goods.newPrice}}</span>
        <span class="o-price">{{goods.oldPrice}}</span>
        <span v-if="goods.discount" class="discount">{{goods.discount}}</span>
      </div>
      <div class="info-other">
        <span>{{goods.columns[0]}}</span>
        <span>{{goods.columns[1]}}</span>
        <span>{{goods.services[goods.services.length-1].name}}</span>
      </div>
      <div>
        <span>
          <!--<img :src="">-->
          <span></span>
        </span>
      </div>
    </div>
</template>

<script>
  export default {
    name: "DetailBaseInfo",
    props: {
      goods: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }
</script>

<style scoped>
  .info-title {
    margin-top: 10px;
    font-size: 20px;
  }
  .info-price {
    margin-top: 10px;
  }
  .n-price {
    font-size: 30px;
    color: var(--color-high-text);
  }
  .o-price {
    margin-left: 10px;
    margin-right: 10px;
    text-decoration: line-through;
  }
  .discount {
    font-size: 20px;
    background-color: var(--color-high-text);
    color: white;
    vertical-align: top;
    border-radius: 5px;
  }
  .info-other {
    padding: 15px 8px 10px 8px;
    display: flex;
    border-bottom: 1px gray solid;
  }
  .info-other span {
    font-size: 12px;
    flex: 1;
    text-align: center;
  }
</style>
