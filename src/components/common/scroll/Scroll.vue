<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        scroll: null,
      }
    },
    mounted() {
      // 1. 创建 BScroll
      this.scroll = new BScroll(this.$refs.wrapper, {
        // 配置项
        probeType: this.probeType,    // 控制是否需要同步更新
        pullUpLoad: this.pullUpLoad,
        click: true,
        mouseWheel: true,
        observeDOM: true
      })
      // 2. 监听滚动
      this.scroll.on("scroll", (position) => {
        // console.log(position);
        this.$emit("contentScroll", position)
      })

      // 3. 触底加载
      this.scroll.on("pullingUp", () => {
        this.$emit("pullingUp")
      })
    },
    methods: {
      // 封装
      // ES6 的默认参数写法
      scrollTo(x, y, time=500) {    // 定位滚动的位置
        this.scroll.scrollTo(x, y, time)
      },
      finishPullUp() {    // 让下拉加载时间可以再次触发
        this.scroll.finishPullUp()
      }
    }
  }
</script>

<style scoped>

</style>
