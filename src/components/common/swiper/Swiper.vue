<template>
  <div id="hy-swiper">
    <div class="swiper" @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd">
      <slot></slot>
    </div>
    <slot name="indicator"></slot>
    <div class="indicator">
       <!-- v-for="(item, index) in slideCount" :key="index"  -->
      <slot name="indicator" v-if="showIndicator && slideCount > 1">
        <div class="indi-item" v-for="(item, index) in slideCount" :key="index" :class="{active: index == currentIndex - 1}"></div>
      </slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Swiper',
  props: {
    // 动画过渡持续时间
    animDuration: {
      type: Number,
      default: 300
    },

    // 轮播的间隔时间
    interval: {
      type: Number,
      default: 3000
    },

    // 设置触发滚动的距离比例
    moveRadio: {
      type: Number,
      default: 0.25
    },

    // 是否显示底部标识
    showIndicator: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      currentIndex: 1,    // 当前轮播图的 index
      slideCount: 0,      // 轮播图数量
      totalWidth: 0,      // 总体宽度
      swiperStyle: {},    // 样式
      scrolling: false    // 滚动状态
    }
  },
  mounted() {
    setTimeout(() => {
      this.handleDOM()

      this.startTime()
    }, 1000)
  },
  methods: {
    // 自动轮播的定时器
    startTime() {
      this.playTime = setInterval(() => {
        this.currentIndex ++
        this.scrollContent(-this.currentIndex * this.totalWidth)
        // console.log(1);
      }, this.interval);
    },
    stopTimer() {
      clearInterval(this.playTime)
    },


    // 轮播到正确的位置
    scrollContent(currentPosition) {
      // console.log(currentPosition);

      // 1. 切换滚动状态
      this.scrolling = true;

      // 2. 开始滚动
      this.swiperStyle.transition = 'transform ' + this.animDuration + 'ms'
      this.setTransfrom(currentPosition)

      // 3. 判断滚动位置
      this.checkPostion()

      // 4. 滚动完成 恢复状态
      this.scrolling = false
    },

    // 判断滚动位置
    checkPostion() {
      setTimeout(() => {
        // 1. 矫正正确的位置
        this.swiperStyle.transition = '0ms'
        if (this.currentIndex >= this.slideCount + 1) {
          this.currentIndex = 1
          this.setTransfrom(-this.currentIndex * this.totalWidth)
        } else if (this.currentIndex <= 0) {
          this.currentIndex = this.slideCount
          this.setTransfrom(-this.currentIndex * this.totalWidth)
        }

        // 2.结束移动后的回调
        this.$emit('transitionEnd', this.currentIndex-1);
      }, this.animDuration)
    },


    // 设置滚动位置
    setTransfrom(position) {
      this.swiperStyle.transform = `translate(${position}px)`;
      this.swiperStyle['-webkit-transform'] = `translate(${position}px)`;
      this.swiperStyle['-ms-transform'] = `translate(${position}px)`;
    },


    // 操作 DOM 前后添加 slide 元素
    handleDOM() {
      // 1. 获取元素
      let swiperEl = document.querySelector(".swiper")
      let slideEls = document.getElementsByClassName("slide")

      // 2. 记录轮播图数量
      this.slideCount = slideEls.length

      // 3.  添加首位元素
      if (this.slideCount > 1) {
        let slideFirst = slideEls[slideEls.length - 1].cloneNode(true)
        let slideLast = slideEls[0].cloneNode(true)
        swiperEl.appendChild(slideLast)
        swiperEl.insertBefore(slideFirst, slideEls[0])

        this.totalWidth = swiperEl.offsetWidth
        this.swiperStyle = swiperEl.style

      }

      // 显示第一个轮播图元素
      this.setTransfrom(-this.totalWidth)
    },


    // 拖动效果
    touchStart(e) {
      // 1. 如果正在移动 直接终止该事件
      if (this.scrolling) return

      // 2. 停止计时器
      this.stopTimer()

      // 3. 保存当前滚动的位置
      // console.log(e);
      this.startX = e.touches[0].pageX
      // console.log(this.startX);
    },

    touchMove(e) {
      // console.log(2);
      // 1. 计算当前的拖动距离
      this.currentX = e.touches[0].pageX
      this.distance = this.currentX - this.startX

      let currentPosition = -this.currentIndex * this.totalWidth
      let movePosition = this.distance + currentPosition

      // 2. 设置当前位置
      this.setTransfrom(movePosition)
    },

    touchEnd() {
      // 1. 获取移动的距离
      let currentMove = Math.abs(this.distance)

      // 2. 判断当前的位置 是否满足移动滚动一次
      if (currentMove === 0) {
        return
      } else if (this.distance > 0 && currentMove > this.totalWidth * this.moveRadio) {
        this.currentIndex --
      } else if (this.distance < 0 && currentMove > this.totalWidth * this.moveRadio) {
        this.currentIndex ++
      }

      // 3. 移动到正确的位置上
      this.scrollContent(-this.currentIndex * this.totalWidth)

      // 4. 开启定时器
      this.startTime()
    },


    // 控制上一个下一个
    previous() {
      this.changeItem(-1)
    },
    next() {
      this.changeItem(1)
    },
    changeItem(num) {

      // 1. 取消定时器
      this.stopTimer()

      // 2. 执行滚动函数
      this.currentIndex += num
      this.scrollContent(-this.currentIndex * this.totalWidth)

      // 3. 打开定时器
      this.startTime()
    }
  }
}
</script>

<style scoped>
  #hy-swiper{
    position: relative;
    overflow: hidden;
  }

  .swiper{
    width: 100%;
    display: flex;
    position: relative;
  }

  .indicator{
    width: 100%;
    display: flex;
    height: 8px;
    justify-content: center;
    position: absolute;
    bottom: 8px;
    right: 0;
    left: 0;
  }

  .indi-item{
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #fff;
    margin: 0 5px;
  }

  .indi-item.active{
    background: red;
  }
</style>
