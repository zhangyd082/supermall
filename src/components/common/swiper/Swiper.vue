<template>
  <div id="hy-swiper">
    <!-- 轮播图片 -->
    <div class="swiper" @touchstart="touchStart($event)" @touchmove="touchMove($event)" @touchend="touchEnd">
      <slot></slot>
    </div>
    <!-- 指示器 -->
    <!-- 相当于用户自定义 -->
    <slot name="indicator"></slot>
    <!-- 相当于系统自带 -->
    <div class="indicator" v-if="showIndicator && slideCount > 1">
      <div v-for="(item, index) in slideCount" :key="index" class="indi-item"
           :class="{active: index === currentIndex - 1}">
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Swiper",
    props: {
      // swiper停留时间
      interval: {
        type: Number,
        default: 3000
      },
      // 是否显示指示器
      showIndicator: {
        type: Boolean,
        default: true
      },
      // swiper滚动动画持续时间
      animDuration: {
        type: Number,
        default: 300
      },
      // 拖动的比例
      moveRatio: {
        type: Number,
        default: 0.25
      }
    },
    data() {
      return {
        slideCount: 0, // 元素个数
        currentIndex: 1, // 当前的index
        totalWidth: 0, // swiper宽度
        scrolling: false, // 是否正在滚动
        swiperStyle: {}, // swiper样式
      }
    },
    mounted() {
      // 1.操作DOM，在前后添加Slide
      setTimeout(() => {
        this.handDom();

        // 2.开启定时器
        this.startTimer()
      }, 100)
    },
    methods: {
      // 定时器操作
      startTimer() {
        this.playTimer = window.setInterval(() => {
          this.currentIndex++;
          this.scrollContent(-this.currentIndex * this.totalWidth)
        }, this.interval)
      },
      stopTimer() {
        window.clearInterval(this.playTimer)
      },

      // 滚动到正确的位置
      scrollContent(currentPosition) {
        // 1.设置正在滚动
        this.scrolling = true

        // 2.开始滚动动画
        this.swiperStyle.transition = 'transform ' + this.animDuration + 'ms'
        this.setTransform(currentPosition)

        // 3.判断滚动到的位置
        this.checkPosition()

        // 4.滚动完成
        this.scrolling = false
      },

      // 设置滚动的位置
      setTransform(position) {
        // ES6中 将字符串变量用${}包起来，配合反单引号完成拼接字符串的功能
        this.swiperStyle.transform = `translate3d(${position}px, 0, 0)`
        this.swiperStyle['-webkit-transform'] = `translate3d(${position}px), 0, 0`;
        this.swiperStyle['-ms-transform'] = `translate3d(${position}px), 0, 0`;
      },

      // 校验正确位置
      checkPosition() {
        window.setTimeout(() => {
          // 1.校验正确的位置
          this.swiperStyle.transition = '0ms';
          if (this.currentIndex >= this.slideCount + 1) {
            this.currentIndex = 1
          } else {
            this.currentIndex = this.slideCount
          }
          this.setTransform(-this.currentIndex * this.totalWidth)

          // 2.结束移动后的回调
          this.$emit('transitionEnd', this.currentIndex - 1)
        }, this.animDuration)
      },

      // 操作DOM,在DOM前后添加Slide
      handDom() {
        // 1.获取要操作的元素
        let swiperEl = document.querySelector('.swiper')
        let slidesEls = swiperEl.getElementsByClassName('slide')

        // 2.保存个数
        this.slideCount = slidesEls.length

        // 3.如果大于1个，前后分别添加一个slide
        if (this.slideCount > 1) {
          let cloneFirst = slidesEls[0].cloneNode(true)
          let cloneLast = slidesEls[this.slideCount - 1].cloneNode(true)
          // 在列表已有节点前插入一个元素
          swiperEl.insertBefore(cloneLast, slidesEls[0])
          swiperEl.appendChild(cloneFirst)
          this.totalWidth = swiperEl.offsetWidth
          this.swiperStyle = swiperEl.style
        }

        // 4.让swiper显示第一个(目前是显示前面添加的最后一个元素)
        console.log(this.totalWidth)
        this.setTransform(-this.totalWidth)
      },

      // 拖动事件的处理
      touchStart(event) {
        // 1.如果正在滚动，不可以拖动
        if (this.scrolling) return

        // 2.停止计时器
        this.stopTimer()

        // 3.保存开始滚动的位置
        this.startX = event.touches[0].pageX
      },
      touchMove(event) {
        // 1.计算出用户拖动的距离
        this.currentX = event.touches[0].pageX
        this.distance = this.currentX - this.startX
        let currentPosition = -this.currentIndex * this.totalWidth
        let moveDistance = this.distance + currentPosition

        // 2.设置当前位置
        this.setTransform(moveDistance)
      },
      touchEnd() {
        // 1.获取移动的距离
        let currentMove = Math.abs(this.distance)

        // 2.判断最终的距离
        if (this.distance === 0) {
          return
        } else if (this.distance > 0 && currentMove > this.totalWidth * this.moveRatio) {
          // 右边移动超过0.5
          this.currentIndex--
        } else if (this.distance < 0 && currentMove > this.totalWidth * this.moveRatio) {
          // 左边移动超过0.5
          this.currentIndex++
        }

        // 3.移动到正确位置
        this.scrollContent(-this.currentIndex * this.totalWidth)

        // 4.移动完成重新开启定时器
        this.startTimer()
      },
    }
  }
</script>

<style scoped>
  #hy-swiper {
    overflow: hidden;
    position: relative;
  }

  .swiper {
    display: flex;
  }

  .indicator {
    display: flex;
    justify-content: center;
    position: absolute;
    width: 100%;
    bottom: 8px;
  }

  .indi-item {
    box-sizing: border-box;
    width: 8px;
    height: 8px;
    border-radius: 4px;
    background-color: #fff;
    line-height: 8px;
    text-align: center;
    font-size: 12px;
    margin: 0 5px;
  }

  .indi-item.active {
    background-color: rgba(212, 62, 46, 1.0);
  }
</style>
