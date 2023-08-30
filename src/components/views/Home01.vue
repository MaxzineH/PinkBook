<script setup>
import { ref, reactive, onMounted } from "vue";
import FallWater from "@/components/common/FallWater.vue"
import Card from "@/components/common/Card.vue"
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css';
import CardData from "@/test/data/card.js"
const cardData = reactive(CardData)
const swiperList = ["关注", "发现"]

const currentIdx = ref(1)
const mySwiper = ref(null)

const onSwiper = (swiper) => {
  mySwiper.value = swiper
}

/**
 * 通过点击头部Tab切换swiper
 * @param idx tab的id
 */
function handelSlideTo(idx) {
  currentIdx.value = idx
  mySwiper.value?.slideTo(idx)
}
/**
 * 通过滑动swiper切换头部Tab
 */
const activeId = ref(0)
const previousId = ref(0)
const lineXCur = ref(0)
const lineXPre = ref(0)
const onSlideChange = (swiper) => {
  handelSlideTo(swiper.activeIndex)

  activeId.value = swiper.activeIndex
  previousId.value = swiper.previousIndex

  const swiperNameChildrenAct = swiperName.value.children[swiper.activeIndex]
  const actData = {
    clientRectLeft: swiperNameChildrenAct.getBoundingClientRect().left,//当前标题距离页面距离浏览器左边的距离
    clientWidth: swiperNameChildrenAct.clientWidth,//当前标题宽度
  }
  lineXCur.value = actData.clientRectLeft - baseData.clientRectLeftOut + (actData.clientWidth - baseData.clientWidthLine) / 2

  const swiperNameChildrenPre = swiperName.value.children[swiper.previousIndex]
  const preData = {
    clientRectLeft: swiperNameChildrenPre.getBoundingClientRect().left,//当前标题距离页面距离浏览器左边的距离
    clientWidth: swiperNameChildrenPre.clientWidth,//当前标题宽度
  }
  lineXPre.value = preData.clientRectLeft - baseData.clientRectLeftOut + (preData.clientWidth - baseData.clientWidthLine) / 2
}

const redLine = ref(null)
const swiperName = ref(null)
/**
 * 计算红线x轴位置
 */
const setLinePosition = (id) => {
  const swiperNameChildrenCurrent = swiperName.value.children[id]
  const currentData = {
    clientRectLeft: swiperNameChildrenCurrent.getBoundingClientRect().left,//当前标题距离页面距离浏览器左边的距离
    clientWidth: swiperNameChildrenCurrent.clientWidth,//当前标题宽度
  }
  const lineX = currentData.clientRectLeft - baseData.clientRectLeftOut + (currentData.clientWidth - baseData.clientWidthLine) / 2
  redLine.value.clientWidth
  redLine.value.style.transform = `translateX(${lineX}px) scaleX(1)`
}

/**
 * 红线过渡时style
 */
const onProgress = (swiper) => {
  // setLinePosition(swiper.activeIndex)
  const prog = swiper.progress.toFixed(2)
  const step = Math.abs(prog * 10 - 5).toFixed(0)
  // console.log(step)
  const scaleValue = ((baseData.clientWidthLine + 30 - step * 6) / baseData.clientWidthLine)
  // console.log(scaleValue)


  const lineX = ref(0)
  const step1 = Math.abs(lineXPre.value - lineXCur.value)
  // console.log(swiper.previousIndex, swiper.activeIndex, prog,swiper)
  if (swiper.previousIndex) {
    if (activeId > previousId) {//往左划
      if (swiper.progress - 0.5 <= 0) {
        lineX.value = lineXCur.value + step1 * prog
        redLine.value.style.transform = `translateX(${lineX.value}px) scaleX(${scaleValue})`
      } else {
        redLine.value.style.transform = `scaleX(${scaleValue})`
      }
    } else {//往右划
      if (swiper.progress - 0.5 >= 0) {
        // console.log(Math.abs('2:',swiper.progress - 0.5))
        lineX.value = lineXPre.value - step1 * prog
        redLine.value.style.transform = `translateX(${lineX.value}px) scaleX(${scaleValue})`
      } else {
        redLine.value.style.transform = `scaleX(${scaleValue})`
      }
    }
    // console.log(lineX.value)
  }
}

let baseData = {}
onMounted(() => {
  /**
   * 通过滑动swiper切换tab标题下的横线
   */
  baseData = {
    clientWidthLine: redLine.value.clientWidth,//红线宽度
    clientRectLeftOut: swiperName.value.getBoundingClientRect().left,//整个swiper标题框距离浏览器左边的距离
  }
  setLinePosition(currentIdx.value)//初始化红线位置

})
</script>

<template>
  <div class="home-container">
    <div class="home-swiper-head">
      <i class="iconfont icon-tiantianquan"></i>
      <div class="swiper-name">
        <div ref="swiperName" class="swiper-name-top">
          <div class="swiper-p" v-for="(item, idx) in swiperList" :key="idx"
            :class="{ 'swiper-active': idx == currentIdx }" @click="handelSlideTo(idx)">{{ item }}</div>
        </div>
        <div ref="redLine" class="red-line"></div>
      </div>
      <i class="iconfont icon-sousuo"></i>
    </div>

    <swiper @swiper='onSwiper' class="home-swiper" :initialSlide="1" :watchSlidesProgress="true"
      @slideChange="onSlideChange" @progress="onProgress">

      <swiper-slide>
        关注
      </swiper-slide>

      <swiper-slide>
        发现
        <!-- <FallWater :imgLength="cardData.length">
          <template v-slot="{ addImageCount }">
            <Card v-for="(item, index) in cardData" :key="index" :cardInfo="item" @addImageCount="addImageCount">
            </Card>
          </template>
        </FallWater> -->
      </swiper-slide>

    </swiper>
  </div>
</template>

<style lang="scss" scoped>
.home-container {
  background-color: rgba(249, 250, 249);
  height: 100vh;

  .home-swiper-head {
    height: 3rem;
    background-color: rgba(255, 255, 255);
    padding: 0 1rem;
    @include flexlr;

    .iconfont {
      font-size: 1.2rem;
      color: rgba(51, 51, 51);
    }

    .iconfont:nth-child(1) {
      color: rgb(248, 230, 28);
    }

    .swiper-name {
      width: 30%;
      position: relative;

      .swiper-name-top {
        width: 100%;
        @include flexlr;
      }

      .swiper-p {
        font-size: 1rem;
        color: rgba(149, 149, 149);
        transition: 0.1s linear;
      }

      .red-line {
        width: 30px;
        height: 2px;
        border-radius: 50%;
        background-color: rgba(255, 34, 61);
        position: absolute;
        bottom: -2px;
        left: 0;
        transition: 0.5s linear;
      }

    }
  }

  .swiper-active {
    font-size: 1.05rem !important;
    color: rgba(51, 51, 51) !important;
    font-weight: bold;
  }

  .home-swiper {
    height: calc(100% - 6rem);
    overflow-y: auto;
  }
}
</style>
