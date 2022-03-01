<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners"/>
    <recommend-view :recommends="recommends"/>
    <feature-view/>
    <tab-control class="tab-control" :titles="['流行', '新歌', '精选']" @tabClick="tabClick"/>

    <goods-list :goods="goods['pop'].list"/>

    <ul>
      <li>测试文字1</li>
      <li>测试文字2</li>
      <li>测试文字3</li>
      <li>测试文字4</li>
      <li>测试文字5</li>
      <li>测试文字6</li>
      <li>测试文字7</li>
      <li>测试文字8</li>
      <li>测试文字9</li>
      <li>测试文字10</li>
      <li>测试文字11</li>
      <li>测试文字12</li>
      <li>测试文字13</li>
      <li>测试文字14</li>
      <li>测试文字15</li>
      <li>测试文字16</li>
      <li>测试文字17</li>
      <li>测试文字18</li>
      <li>测试文字19</li>
      <li>测试文字20</li>
      <li>测试文字21</li>
      <li>测试文字22</li>
      <li>测试文字23</li>
      <li>测试文字24</li>
      <li>测试文字25</li>
      <li>测试文字26</li>
      <li>测试文字27</li>
      <li>测试文字28</li>
      <li>测试文字29</li>
      <li>测试文字30</li>
      <li>测试文字31</li>
      <li>测试文字32</li>
      <li>测试文字33</li>
      <li>测试文字34</li>
      <li>测试文字35</li>
      <li>测试文字36</li>
      <li>测试文字37</li>
      <li>测试文字38</li>
      <li>测试文字39</li>
      <li>测试文字40</li>
      <li>测试文字41</li>
      <li>测试文字42</li>
      <li>测试文字43</li>
      <li>测试文字44</li>
      <li>测试文字45</li>
      <li>测试文字46</li>
      <li>测试文字47</li>
      <li>测试文字48</li>
      <li>测试文字49</li>
      <li>测试文字50</li>
      <li>测试文字51</li>
      <li>测试文字52</li>
      <li>测试文字53</li>
      <li>测试文字54</li>
      <li>测试文字55</li>
      <li>测试文字56</li>
      <li>测试文字57</li>
      <li>测试文字58</li>
      <li>测试文字59</li>
      <li>测试文字60</li>
      <li>测试文字61</li>
      <li>测试文字62</li>
      <li>测试文字63</li>
      <li>测试文字64</li>
      <li>测试文字65</li>
      <li>测试文字66</li>
      <li>测试文字67</li>
      <li>测试文字68</li>
      <li>测试文字69</li>
      <li>测试文字70</li>
      <li>测试文字71</li>
      <li>测试文字72</li>
      <li>测试文字73</li>
      <li>测试文字74</li>
      <li>测试文字75</li>
      <li>测试文字76</li>
      <li>测试文字77</li>
      <li>测试文字78</li>
      <li>测试文字79</li>
      <li>测试文字80</li>
      <li>测试文字81</li>
      <li>测试文字82</li>
      <li>测试文字83</li>
      <li>测试文字84</li>
      <li>测试文字85</li>
      <li>测试文字86</li>
      <li>测试文字87</li>
      <li>测试文字88</li>
      <li>测试文字89</li>
      <li>测试文字90</li>
      <li>测试文字91</li>
      <li>测试文字92</li>
      <li>测试文字93</li>
      <li>测试文字94</li>
      <li>测试文字95</li>
      <li>测试文字96</li>
      <li>测试文字97</li>
      <li>测试文字98</li>
      <li>测试文字99</li>
      <li>测试文字100</li>
    </ul>
  </div>
</template>

<script>
  import HomeSwiper from './childCompon/HomeSwiper.vue'
  import RecommendView from './childCompon/RecommendView.vue'
  import FeatureView from './childCompon/FeatureView.vue'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'

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
        currentIndex: 'pop'
      }
    },
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodsList
    },
    created() {
      // 1. 请求多个数据
      this.getHomeMultidata()

      // 2. 请求商品数据
      this.getHomeGoods("pop")
      this.getHomeGoods("new")
      this.getHomeGoods("sell")
    },
    methods: {
      // 事件监听的相关方法
      tabClick(index) {
        this.currentIndex = index
        switch (currentIndex) {
          case 1:
            this.currentIndex = 'new'
            break;

          default:
            break;
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
        })
      }



    }
  }
</script>

<style scoped>
  #home{
    padding-top: 44px;
  }
  .home-nav{
    background-color: var(--color-tint);
    color: #fff;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 3;
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
</style>
