<template>
    <div id="detail">
      <detail-nav-bar class="detail-nav" @titleClick="titleClick"/>
      <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll">
        <detail-swiper :topImages="topImages"/>
        <detail-base-info :goods="goods"></detail-base-info>
        <p>商品id：{{iid}}</p>
        <div><p>商品id：{{iid}}</p></div>
        <div><p>商家信息</p></div>
        <div v-for="item in 100"><p>balabala</p></div>
      </scroll>
      <back-top @click.native="backClick" v-show="isShowBackTop"/>
      <detail-bottom-bar @addCart="addToCart"/>
      <!--<toast :message="message" :show="show"/>-->
    </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import DetailBaseInfo from './childComps/DetailBaseInfo'
  import DetailBottomBar from './childComps/DetailBottomBar'

  import Scroll from 'components/common/scroll/Scroll'
  // import Toast from 'components/common/toast/Toast'

  import {getDetail, Goods} from "network/detail";
  import {backTopMixin} from "common/mixin";

  export default {
    name: "Detail",
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      Scroll,
      DetailBottomBar,
      // Toast
    },
    mixins: [backTopMixin],
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        themeTopYs: [0, 300, 600, 1000],
        // message: '',
        // show: false
      }
    },
    methods: {
      titleClick(index) {
        console.log(index);
        this.$refs.scroll.scrollTo(0,-this.themeTopYs[index],200)
      },
      contentScroll(position) {
        //判断BackTop是否显示
        this.isShowBackTop = -position.y > 1000
      },
      addToCart() {
        const product = {}
        product.iid = this.iid
        product.image = this.topImages[0]
        product.title = this.goods.title
        product.desc = this.goods.desc
        product.price = this.goods.realPrice

        // this.$store.commit('addCart',product)
        this.$store.dispatch('addCart',product).then(res => {
          // this.show = true
          // this.message = res
          //
          // setTimeout(() => {
          //   this.show = false
          //   this.message = ''
          // }, 1500)
          // console.log(res);
          this.$toast.show(res,2000)
        })
        // console.log(product);
      }
    },
    created() {
      this.iid = this.$route.params.iid
      //根据iid请求详情数据
      getDetail(this.iid).then(res => {
        console.log(res);
        //获取顶部图片
        // this.topImages = res.result.itemInfo.topImages
        //模拟网络请求
        const res_data ={
          topImages:[
            'http://pic.rmb.bdstatic.com/fcd9555bd33f379035bcc05e71be30d2.jpeg',
            'http://img4.a0bi.com/upload/ttq/20170715/1500081601921.png',
            'http://img5.imgtn.bdimg.com/it/u=2164250755,1128815926&fm=26&gp=0.jpg'
          ],
          itemInfo: {
            title: '【品牌】2019新上市balabalabala......',
            desc: '新款上市',
            price: '￥9.90',
            oldPrice: '￥10.0',
            discountDesc: '活动价',
            lowNowPrice: 9.9
          },
          columns: ['销量999','收藏33人','默认快递'],
          shopInfo: {
            services: [
              {
                name: '七天内发货'
              }
            ]
          }
        }
        this.topImages=res_data.topImages
        //获取商品信息
        // this.goods = new Goods(res.result.itemInfo, res.result.columns, res.result.shopInfo.services)
        this.goods = new Goods(res_data.itemInfo, res_data.columns, res_data.shopInfo.services)
      })
    }
  }
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }
  .content {
    background-color: #fff;
    height: calc(100% - 44px - 58px);
  }
  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }
</style>
