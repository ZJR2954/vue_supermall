<template>
    <div id="home">
      <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
      <tab-control :titles="['流行','新款','精选']"
                   @tabClick="tabClick"
                   ref="tabControl1"
                   class="tab-control" v-show="isTabFixed"/>
      <scroll class="content" ref="scroll"
              :probe-type="3"
              @scroll="contentScroll"
              :pull-up-load="true"
              @pullingUp="loadMore">
        <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
        <home-recommend-view :recommends="recommends"/>
        <home-feature-view/>
        <tab-control :titles="['流行','新款','精选']"
                     @tabClick="tabClick"
                     ref="tabControl"/>
        <goods-list :goods="showGoods"/>
      </scroll>
      <back-top @click.native="backClick" v-show="isShowBackTop"/>

    </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper'
  import HomeRecommendView from './childComps/HomeRecommendView'
  import HomeFeatureView from './childComps/HomeFeatureView'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'
  import Scroll from 'components/common/scroll/Scroll'
  // import BackTop from 'components/content/backTop/BackTop'

  import {getHomeMultidata,getHomeGoods} from "network/home";
  import {debounce} from 'common/utils'
  import {backTopMixin} from "common/mixin";

  export default {
    name: "Home",
    components: {
      HomeSwiper,
      HomeRecommendView,
      HomeFeatureView,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      // BackTop
    },
    mixins: [backTopMixin],
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0,list: []},
          'new': {page: 0,list: []},
          'sell': {page: 0,list: []}
        },
        currentType: 'pop',
        // isShowBackTop: false,
        tabOffsetTop: 0,
        isTabFixed: false,
        saveY: 0
      }
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    methods: {
      /*
      事件监听相关的方法
       */
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
                break;
          case 1:
            this.currentType = 'new'
            break;
          case 2:
            this.currentType = 'sell'
            break;
        }
        this.$refs.tabControl.currentIndex = index;
        this.$refs.tabControl1.currentIndex = index;
      },
      // backClick() {
      //   this.$refs.scroll.scrollTo(0,0,500)
      // },
      contentScroll(position) {
        //判断BackTop是否显示
        this.isShowBackTop = -position.y > 1000
        //判断tabControl是否吸顶
        this.isTabFixed = (-position.y) > this.tabOffsetTop

      },
      loadMore() {
        this.getHomeGoods(this.currentType)
        // this.$refs.scroll.scroll.refresh()
      },
      swiperImageLoad() {
        // console.log(this.$refs.tabControl.$el.offsetTop);
        this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop

      },

      /*
      网络请求相关的方法
       */
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          console.log(res);
          // this.banners = res.data.banner.list;
          // this.recommends = res.data.recommend.list;
          //模拟网络获取
          this.banners = [
            {
              link: 'http://5b0988e595225.cdn.sohucs.com/images/20171115/a7c5f199b6594321b26ae61224562b93.jpeg',
              image: 'http://5b0988e595225.cdn.sohucs.com/images/20171115/a7c5f199b6594321b26ae61224562b93.jpeg'
            },
            {
              link: 'http://img.mp.itc.cn/upload/20161223/b717f6c4ab9e4436bd0ff73aa902494e_th.jpeg',
              image: 'http://img.mp.itc.cn/upload/20161223/b717f6c4ab9e4436bd0ff73aa902494e_th.jpeg'
            },
            {
              link: 'http://5b0988e595225.cdn.sohucs.com/images/20170824/71a84dcadc9f4f3b9a4684430c7e5e60.jpeg',
              image: 'http://5b0988e595225.cdn.sohucs.com/images/20170824/71a84dcadc9f4f3b9a4684430c7e5e60.jpeg'
            },
            {
              link: 'http://img.mp.itc.cn/upload/20170311/fa4d80437c354c4187ce7e242d01c24d_th.jpg',
              image: 'http://img.mp.itc.cn/upload/20170311/fa4d80437c354c4187ce7e242d01c24d_th.jpg'
            },
          ]
          this.recommends = [
            {
              link: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264290737&di=62fdd49e3e2900a674aa8b371480d3ec&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201701%2F30%2F20170130181206_x8ScK.thumb.700_0.jpeg',
              image: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264290737&di=62fdd49e3e2900a674aa8b371480d3ec&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201701%2F30%2F20170130181206_x8ScK.thumb.700_0.jpeg',
              title: 'title1'
            },
            {
              link: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264504545&di=a9a934936d3dc49708ba00403915441c&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201703%2F03%2F20170303150812_kNU3J.thumb.700_0.jpeg',
              image: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264504545&di=a9a934936d3dc49708ba00403915441c&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201703%2F03%2F20170303150812_kNU3J.thumb.700_0.jpeg',
              title: 'title2'
            },
            {
              link: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264290737&di=62fdd49e3e2900a674aa8b371480d3ec&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201701%2F30%2F20170130181206_x8ScK.thumb.700_0.jpeg',
              image: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264290737&di=62fdd49e3e2900a674aa8b371480d3ec&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201701%2F30%2F20170130181206_x8ScK.thumb.700_0.jpeg',
              title: 'title3'
            },
            {
              link: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264504545&di=a9a934936d3dc49708ba00403915441c&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201703%2F03%2F20170303150812_kNU3J.thumb.700_0.jpeg',
              image: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568264504545&di=a9a934936d3dc49708ba00403915441c&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201703%2F03%2F20170303150812_kNU3J.thumb.700_0.jpeg',
              title: 'title4'
            }
          ]
        })

      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          // console.log(res);
          // this.goods[type].list.push(...res.data.list)
          // this.goods[type].page += 1
          //模拟网络获取
          const res_data_list1 = [
            {
              iid:'001',
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568285887169&di=908e642d4890c2c5513f0e24ddc2fe65&imgtype=0&src=http%3A%2F%2Fpics5.baidu.com%2Ffeed%2Fbf096b63f6246b6075a90c396c1ba748530fa2c4.jpeg%3Ftoken%3Df9a2d8b1014c33e7291afe11b099276c%26s%3DBD328719595267D656405FD80300D0B5'
              },
              title: 'pop1',
              price: 9.9,
              cfav: 108
            },
            {
              iid:'002',
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568286342900&di=d8f2f8cac03b030652ecf9f31d722753&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn%2Fw640h850%2F20180303%2Fcc7b-fwnpcnt6632711.jpg'
              },
              title: 'pop2',
              price: 9.8,
              cfav: 99
            },
            {
              iid:'003',
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568289118169&di=2559b2346f8e75170c5a0901763b9c11&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201412%2F17%2F20141217121646_LLz2u.thumb.700_0.jpeg'
              },
              title: 'pop3',
              price: 9.8,
              cfav: 99
            },
            {
              iid:'004',
              show:{
                img: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793081912,2696674497&fm=26&gp=0.jpg'
              },
              title: 'pop4',
              price: 9.9,
              cfav: 108
            },
            {
              iid:'005',
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568285887169&di=908e642d4890c2c5513f0e24ddc2fe65&imgtype=0&src=http%3A%2F%2Fpics5.baidu.com%2Ffeed%2Fbf096b63f6246b6075a90c396c1ba748530fa2c4.jpeg%3Ftoken%3Df9a2d8b1014c33e7291afe11b099276c%26s%3DBD328719595267D656405FD80300D0B5'
              },
              title: 'pop5',
              price: 9.9,
              cfav: 108
            },
            {
              iid:'006',
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568286342900&di=d8f2f8cac03b030652ecf9f31d722753&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn%2Fw640h850%2F20180303%2Fcc7b-fwnpcnt6632711.jpg'
              },
              title: 'pop6',
              price: 9.8,
              cfav: 99
            },
            {
              iid:'007',
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568289118169&di=2559b2346f8e75170c5a0901763b9c11&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201412%2F17%2F20141217121646_LLz2u.thumb.700_0.jpeg'
              },
              title: 'pop7',
              price: 9.8,
              cfav: 99
            },
            {
              iid:'008',
              show:{
                img: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793081912,2696674497&fm=26&gp=0.jpg'
              },
              title: 'pop8',
              price: 9.9,
              cfav: 108
            }
          ]
          const res_data_list2 = [
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568289118169&di=2559b2346f8e75170c5a0901763b9c11&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201412%2F17%2F20141217121646_LLz2u.thumb.700_0.jpeg'
              },
              title: 'new1',
              price: 9.8,
              cfav: 99
            },
            {
              show:{
                img: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793081912,2696674497&fm=26&gp=0.jpg'
              },
              title: 'new2',
              price: 9.9,
              cfav: 108
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568285887169&di=908e642d4890c2c5513f0e24ddc2fe65&imgtype=0&src=http%3A%2F%2Fpics5.baidu.com%2Ffeed%2Fbf096b63f6246b6075a90c396c1ba748530fa2c4.jpeg%3Ftoken%3Df9a2d8b1014c33e7291afe11b099276c%26s%3DBD328719595267D656405FD80300D0B5'
              },
              title: 'new3',
              price: 9.9,
              cfav: 108
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568286342900&di=d8f2f8cac03b030652ecf9f31d722753&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn%2Fw640h850%2F20180303%2Fcc7b-fwnpcnt6632711.jpg'
              },
              title: 'new4',
              price: 9.8,
              cfav: 99
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568289118169&di=2559b2346f8e75170c5a0901763b9c11&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201412%2F17%2F20141217121646_LLz2u.thumb.700_0.jpeg'
              },
              title: 'new5',
              price: 9.8,
              cfav: 99
            },
            {
              show:{
                img: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793081912,2696674497&fm=26&gp=0.jpg'
              },
              title: 'new6',
              price: 9.9,
              cfav: 108
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568285887169&di=908e642d4890c2c5513f0e24ddc2fe65&imgtype=0&src=http%3A%2F%2Fpics5.baidu.com%2Ffeed%2Fbf096b63f6246b6075a90c396c1ba748530fa2c4.jpeg%3Ftoken%3Df9a2d8b1014c33e7291afe11b099276c%26s%3DBD328719595267D656405FD80300D0B5'
              },
              title: 'new7',
              price: 9.9,
              cfav: 108
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568289118169&di=2559b2346f8e75170c5a0901763b9c11&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201412%2F17%2F20141217121646_LLz2u.thumb.700_0.jpeg'
              },
              title: 'new8',
              price: 9.8,
              cfav: 99
            }
          ]
          const res_data_list3 = [
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568285887169&di=908e642d4890c2c5513f0e24ddc2fe65&imgtype=0&src=http%3A%2F%2Fpics5.baidu.com%2Ffeed%2Fbf096b63f6246b6075a90c396c1ba748530fa2c4.jpeg%3Ftoken%3Df9a2d8b1014c33e7291afe11b099276c%26s%3DBD328719595267D656405FD80300D0B5'
              },
              title: 'sell1',
              price: 9.9,
              cfav: 108
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568286342900&di=d8f2f8cac03b030652ecf9f31d722753&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn%2Fw640h850%2F20180303%2Fcc7b-fwnpcnt6632711.jpg'
              },
              title: 'sell2',
              price: 9.8,
              cfav: 99
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568289118169&di=2559b2346f8e75170c5a0901763b9c11&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201412%2F17%2F20141217121646_LLz2u.thumb.700_0.jpeg'
              },
              title: 'sell3',
              price: 9.8,
              cfav: 99
            },
            {
              show:{
                img: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793081912,2696674497&fm=26&gp=0.jpg'
              },
              title: 'sell4',
              price: 9.9,
              cfav: 108
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568285887169&di=908e642d4890c2c5513f0e24ddc2fe65&imgtype=0&src=http%3A%2F%2Fpics5.baidu.com%2Ffeed%2Fbf096b63f6246b6075a90c396c1ba748530fa2c4.jpeg%3Ftoken%3Df9a2d8b1014c33e7291afe11b099276c%26s%3DBD328719595267D656405FD80300D0B5'
              },
              title: 'sell5',
              price: 9.9,
              cfav: 108
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568286342900&di=d8f2f8cac03b030652ecf9f31d722753&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn%2Fw640h850%2F20180303%2Fcc7b-fwnpcnt6632711.jpg'
              },
              title: 'sell6',
              price: 9.8,
              cfav: 99
            },
            {
              show:{
                img: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568289118169&di=2559b2346f8e75170c5a0901763b9c11&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201412%2F17%2F20141217121646_LLz2u.thumb.700_0.jpeg'
              },
              title: 'sell7',
              price: 9.8,
              cfav: 99
            },
            {
              show:{
                img: 'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793081912,2696674497&fm=26&gp=0.jpg'
              },
              title: 'sell8',
              price: 9.9,
              cfav: 108
            }
          ]
          // this.goods[type].list.push(...res_data_list1)
          this.goods['pop'].list.push(...res_data_list1)
          this.goods['new'].list.push(...res_data_list2)
          this.goods['sell'].list.push(...res_data_list3)
          this.goods[type].page += 1

          this.$refs.scroll.finishPullUp()
        })
      }

    },
    created() {
      this.getHomeMultidata()

      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')

    },
    mounted() {
      const refresh = debounce(this.$refs.scroll.refresh,500)
      //全局事件总线
      this.$bus.$on('itemImageLoad', () => {
        // console.log('======');
        refresh()
      })
    },
    activated() {
      this.$refs.scroll.scrollTo(0,this.saveY,1)
      this.$refs.scroll.refresh()
      // console.log(this.saveY);
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY()
      // console.log(this.saveY);
    }
  }
</script>

<style scoped>
  #home {
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /*position: fixed;*/
    /*left: 0;*/
    /*right: 0;*/
    /*top: 0;*/
    /*z-index: 9;*/
  }
  .content {
    /*height: calc(100% - 93px );*/
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
  /*.content {*/
    /*height: calc(100% - 93px );*/
    /*overflow: hidden;*/
  /*}*/
  .tab-control {
    position: relative;
    z-index: 9;
  }
</style>
