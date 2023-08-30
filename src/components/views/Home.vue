<script setup>
import { ref, reactive, onMounted } from "vue";
import FallWater from "@/components/common/FallWater.vue"
import Card from "@/components/common/Card.vue"
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css';
import CardData from "@/test/data/card.js"
const cardData = reactive(CardData)
const swiperList = reactive(["特别关注", "发现", "苏州城", "其他"])

const currentIdx = ref(0)
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
const onSlideChange = (swiper) => {
  handelSlideTo(swiper.activeIndex)
}

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

  //平移

  // redLine.value.style.transform = `${baseData.offsetLeftLineCur + (baseData.clientWidthActiveName - baseData.clientWidthLine) / 2}px`
  //拉伸
}

// let baseData = {}
const redLine = ref(null)
const swiperName = ref(null)
let widthList = []
let posList = []

onMounted(() => {
  /**
   * 通过滑动swiper切换tab标题下的横线
   */
  // baseData = {
  //   offsetLeftLineCur: swiperName.value.children[currentIdx.value].offsetLeft,//当前红线偏移量
  //   clientWidthStart: swiperName.value.children[0].clientWidth,//红线初始位置
  //   clientWidthActiveName: swiperName.value.children[currentIdx.value].clientWidth,//当前swiper name的宽度
  //   clientWidthLine: redLine.value.clientWidth,//红线宽度
  //   clientRectLeftOut: swiperName.value.getBoundingClientRect().left,//整个swiper标题框距离浏览器左边的距离
  // }
  // console.log(baseData.clientWidthStart)
  // redLine.value.style.transform = `translateX(${(baseData.clientWidthStart+20- baseData.clientWidthLine) /2}px)`
  // setLinePosition(currentIdx.value)//初始化红线位置

  let children = swiperName.value.children
  for (let i = 0; i < children.length; i++) {
    widthList.push(children[i].clientWidth)
  }
  for (let i = 0; i < widthList.length; i++) {
    const frontWidth = (i - 1) >= 0 ? widthList[i - 1] : 0
    posList.push(widthList[i] / 2 + frontWidth)
  }
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
        <div ref="redLine" class="red-line-div">
          <div class="red-line"></div>
        </div>
      </div>
      <i class="iconfont icon-sousuo"></i>
    </div>

    <swiper @swiper='onSwiper' class="home-swiper" :initialSlide="currentIdx" :watchSlidesProgress="true"
      @slideChange="onSlideChange" @progress="onProgress">

      <swiper-slide>
        特别关注
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

      <swiper-slide>
        苏州城
      </swiper-slide>
      <swiper-slide>
        其他
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
      position: relative;
      display: flex;
      justify-content: center;

      .swiper-name-top {
        width: 100%;
        @include flexlr;
      }

      .swiper-p {
        padding: 0 10px;
        font-size: 1rem;
        color: rgba(149, 149, 149);
        transition: 0.1s linear;
        box-sizing: border-box;
      }

      .red-line-div {
        position: absolute;
        bottom: -2px;
        left: 0;
        transition: 0.5s linear;
      }

      .red-line {
        width: 30px;
        height: 2px;
        border-radius: 3px;
        background-color: rgba(255, 34, 61);
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
