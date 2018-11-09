<template>
  <div class="slide" ref="slide">
    <div class="slide-group" ref="slideGroup" v-if="slides.length">
      <div class="slide-item" v-for="(item,index) in slides"
           @touchstart="touchStart"
           @touchmove="touchMove"
           @touchend="touchEnd"
           :key="index">
        <a :href="item.linkUrl">
          <img :src="item.picUrl">
        </a>
      </div>
    </div>
    <div class="dots">
      <span class="dot" :class="{active:currentIndex === index}" v-for="(item,index) in slides.length"></span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "slide",
    props: {
      slides: {
        type: Array,
        default: []
      },
      interval: {
        type: Number,
        default: 2500
      },
      speed: {
        type: Number,
        default: 400
      }
    },
    data() {
      return {
        currentIndex: 0
      }
    },
    created() {
      this.flag = true
    },
    computed: {
      current() {
        return this.currentIndex
      },
      next() {
        return this.currentIndex + 1 >= this.slides.length ? 0 : this.currentIndex + 1
      },
      prev() {
        return this.currentIndex - 1 < 0 ? this.slides.length - 1 : this.currentIndex - 1
      }
    },
    mounted() {
      this.autoPlay()
    },
    updated() {
      if (this.flag) {
        this.init()
        this.flag = false
      }
    },
    methods: {
      init() {
        this.$width = this.$refs.slide.clientWidth
        this.$refs.slideGroup.style.width = this.slides.length * this.$width + 'px'
        for (let i = 0; i < this.slides.length; i++) {
          this.$refs.slideGroup.children[i].style.width = this.$width + 'px'
          this.$refs.slideGroup.children[i].style.left = -this.$width * i + 'px'
          this.$refs.slideGroup.children[i].style.transform = `translateX(${this.$width}px)`
        }
        this.$refs.slideGroup.children[0].style.transform = `translateX(0px)`
        this.$refs.slideGroup.children[this.slides.length - 1].style.transform = `translateX(${-this.$width}px)`
      },
      translateX(index, time, dis) {
        this.$refs.slideGroup.children[index].style.transition = `transform ${time}ms ease`
        this.$refs.slideGroup.children[index].style.transform = `translateX(${dis}px)`
      },
      touchStart(e) {
        // e.preventDefault()
        this.startX = e.changedTouches[0].clientX
        this.isMove = false

        this.stop()
      },
      touchMove(e) {
        this.diff = e.changedTouches[0].clientX - this.startX
        this.translateX(this.current, 0, this.diff)
        this.translateX(this.next, 0, this.diff + this.$width)
        this.translateX(this.prev, 0, this.diff - this.$width)
        this.isMove = true
      },
      touchEnd() {
        if (this.isMove && Math.abs(this.diff) >= this.$width / 5) {
          if (this.diff > 0) {
            // 上一张
            this.translateX(this.prev, this.speed, 0)
            this.translateX(this.current, this.speed, this.$width)
            this.translateX(this.next, 0, this.$width)
            this.currentIndex--
          } else {
            // 下一张
            this.translateX(this.prev, 0, this.$width)
            this.translateX(this.current, this.speed, -this.$width)
            this.translateX(this.next, this.speed, 0)
            this.currentIndex++
          }
        } else {
          // 复原
          this.translateX(this.current, this.speed, 0)
          this.translateX(this.next, this.speed, this.$width)
          this.translateX(this.prev, this.speed, -this.$width)
        }
        // 边界处理
        if (this.currentIndex < 0) {
          this.currentIndex = this.slides.length - 1
        } else if (this.currentIndex >= this.slides.length) {
          this.currentIndex = 0
        }

        this.autoPlay()
      },
      autoPlay() {
        this.stop()
        this.timer = setInterval(() => {
          for (let i = 0; i < this.slides.length; i++) {
            if (i !== this.current) {
              this.translateX(i, 0, this.$width)
            }
          }
          this.translateX(this.current, this.speed, -this.$width)
          this.translateX(this.next, this.speed, 0)
          this.currentIndex++
          if (this.currentIndex >= this.slides.length) {
            this.currentIndex = 0
          }
        }, this.interval)
      },
      stop() {
        clearInterval(this.timer)
      }
    }
  }
</script>

<style lang="scss">
  .slide {
    width: calc(100% - 10px);
    margin: 20px auto 0;
    overflow: hidden;
    position: relative;
    /*overflow:hidden;隐藏不成功,尤其是圆角时;试试添加z-index:1;*/
    z-index: 1;
    background: #ccc;
    border-radius: 10px;
    -webkit-tap-highlight-color: transparent;
    .slide-group {
      .slide-item {
        position: relative;
        float: left;
        a {
          overflow: hidden;
          display: block;
          width: 100%;
          text-decoration: none;
          img {
            display: block;
            width: 100%;
          }
        }
      }
    }
    .dots {
      position: absolute;
      bottom: 10px;
      left: 0;
      right: 0;
      height: 6px;
      display: flex;
      align-items: center;
      justify-content: center;
      .dot {
        width: 6px;
        height: 6px;
        background: #fff;
        margin-right: 6px;
        border-radius: 6px;
        transition: all 0.3s ease;
        &.active {
          width: 12px;
          background: red;
        }
        &:last-child {
          margin-right: 0;
        }
      }
    }
  }
</style>
