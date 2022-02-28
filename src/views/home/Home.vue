<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <home-swiper :banners="banners"/>

    <recommend-view :recommends="recommends"/>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'
  import HomeSwiper from './childCompon/HomeSwiper.vue'
  import RecommendView from './childCompon/RecommendView.vue'

  import {getHomeMultidata} from 'network/home'

  export default {
    name: "Home",
    data() {
      return {
        banners: [],
        recommends: []
      }
    },
    components: {
      NavBar,
      HomeSwiper,
      RecommendView
    },
    created() {
      // 1. 请求多个数据
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
        // console.log(this.banners);
      })
    }
  }
</script>

<style scoped>
  .home-nav{
    background-color: var(--color-tint);
    color: #fff;
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
</style>
