<template>
    <div class="wrapper" ref="aaa">
      <!--<h2>分类</h2>-->
      <ul class="content">
        <li v-for="item in 200">分类列表{{item}}</li>
      </ul>
    </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "Category",
    data() {
      return {
        scroll: null
      }
    },
    mounted() {
      this.scroll = new BScroll(document.querySelector('.wrapper'),{
        probeType: 3,
        pullUpLoad: true
      })
      this.scroll.on('scroll',(position) => {
        // console.log(position);
      })
      this.scroll.on('pullingUp', () => {
        console.log('上拉加载更多');
      })
    }
  }
</script>

<style scoped>
  .wrapper {
    height: 150px;
    background-color: greenyellow;

    overflow: hidden;
    /*overflow-y: scroll;*/
  }
</style>
