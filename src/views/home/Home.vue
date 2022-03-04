<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

<!-- @pullingUp="loadMore" -->
    <scroll class="wrapper"
            ref="scroll"
            @contentScroll="contentScroll"

            :probe-type="3"
            :pull-up-load="true">
      <home-swiper :banners="banners"/>
      <recommend-view :recommends="recommends"/>
      <feature-view/>
      <tab-control class="tab-control"
                  :titles="['流行', '新款', '精选']"
                  @tabClick="tabClick"/>
      <goods-list :goods="showGoods"/>
    </scroll>

    <back-top @click.native="backClick" v-show="isBackShow"/>

  </div>
</template>

<script>
  import HomeSwiper from './childCompon/HomeSwiper.vue'
  import RecommendView from './childCompon/RecommendView.vue'
  import FeatureView from './childCompon/FeatureView.vue'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'

  import { getHomeMultidata, getHomeGoods } from 'network/home'

  export default {
    name: "Home",
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        currentType: 'pop',
        isBackShow: false
      }
    },
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    created() {
      // 1. 请求多个数据
      this.getHomeMultidata()

      // 2. 请求商品数据
      this.getHomeGoods("pop")
      this.getHomeGoods("new")
      this.getHomeGoods("sell")
    },
    mounted() {
      // 4. 引入防抖
      const refresh = this.debounce(this.$refs.scroll.refresh)

      // 3. 监听 goodsItem 中的图片加载
      this.$bus.$on("itemImageLoad", () => {
        refresh()
      })
    },
    destroyed() {
      this.$bus.$off()
    },
    methods: {
      // 事件监听的相关方法
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
      },
      backClick() {
        // this.$refs.scroll.scroll.scrollTo(0, 0, 500)    // 调用 BScroll 对象中的 scrollTo 方法 前两个参数：跳转到的位置  第三个参数：跳转时间(选填)
        this.$refs.scroll.scrollTo(0, 0)
      },
      contentScroll(position) {
        this.isBackShow = (-position.y) > 1000
      },
      // loadMore() {
      //   this.getHomeGoods(this.currentType)
      // },
      debounce(func, delay=300) {     // 防抖的封装函数
        let timer = null
        return function (...args) {
          if (timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this, args)
          }, delay)
        }
      },


      // 网络请求的相关方法
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page ++

          // this.$refs.scroll.finishPullUp()
        })
      }



    }
  }
</script>

<style scoped>
  #home{
    padding-top: 44px;
    height: 100vh;
    box-sizing: border-box;
  }
  .home-nav{
    background-color: var(--color-tint);
    color: #fff;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
  }
  .swiper{
    position: relative;
  }

  .hy-swiper .swiper-poster{
    width: 100%;
    flex-shrink: 0;
  }
  .hy-swiper .swiper-poster img{
    width: 100%;
    vertical-align: middle;
  }

  .tab-control {
    position: sticky;
    top: 44px;
  }

  .wrapper{
    height: calc(100vh - 93px);
  }
</style>
