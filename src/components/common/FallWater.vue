<script setup>
import { reactive, ref, onMounted } from 'vue'

let fallColumn = ref(2)//瀑布流列数
const fallContent = ref(null)

/**
 * 设置瀑布流内部组件偏移距离
 */
function setPositions() {
  let fallArray = new Array(fallColumn.value).fill(0) // 创建一个[0,0]的数组
  let fallWidth = ref(fallContent.value.clientWidth / fallColumn.value) //瀑布流每列宽度，内部组件x轴偏移量
  for (let i = 0; i < fallContent.value.children.length; i++) {
    let waterComp = fallContent.value.children[i]
    let waterCompHeight = waterComp.clientHeight
    // console.log(waterCompHeight)
    // console.log(i + ':' + fallArray)
    if (fallArray[0] <= fallArray[1]) {
      waterComp.style.transform = `translate(${fallWidth.value * (fallColumn.value - 2)}px,${fallArray[0]}px)`
      fallArray[0] += waterCompHeight
    } else if (fallArray[0] > fallArray[1]) {
      waterComp.style.transform = `translate(${fallWidth.value * (fallColumn.value - 1)}px,${fallArray[1]}px)`
      fallArray[1] += waterCompHeight
    }
  }
}

/**
*图片加载完毕计数器 
 */
const imgCount = ref(0)
const props = defineProps(['imgLength'])

const addImageCount = () => {
  imgCount.value++
  // console.log(imgCount.value,props.imgLength)
  if (props.imgLength === imgCount.value) {
    setPositions();
  }
}

onMounted(() => {
  let timerId = null;
  window.onresize = function () {
    if (timerId) {
      clearTimeout(timerId)
    }
    timerId = setTimeout(setPositions, 300);
  }
})
</script>

<template>
  <div class="fall-content" ref="fallContent">
    <slot :addImageCount="addImageCount"></slot>
  </div>
</template>

<style lang="scss" scoped>
.fall-content {
  width: 100vw;
  height: 100vh;
  padding: 0;
  margin: 0;
  overflow-y: auto;
  overflow-x: hidden;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  // display: grid;
  // grid-template-columns: repeat(2, 1fr);
  // grid-gap: 10px;
  // grid-template-rows: masonry;/* 仅支持firefox */

  // .fall-col {
  //   width: 50vw;
  // }

  .water {
    position: absolute;
    top: 0;
    left: 0;
    width: 50%;
    transition: 0.3s ease;
    counter-increment: item-counter;
  }

  .water::before {
    position: absolute;
    top: 0;
    left: 0;
    font-size: 13px;
    width: 2em;
    height: 2em;
    text-align: center;
    background-color: #000;
    color: #fff;
    content: counter(item-counter);
  }

  .water:nth-child(3n) {
    background-color: rgb(127, 255, 212);
  }

  .water:nth-child(3n+1) {
    background-color: rgb(242, 255, 127);
  }

  .water:nth-child(3n+2) {
    background-color: rgb(127, 150, 255);
  }

  .water:nth-child(3n) {
    height: 60vh;
  }

  .water:nth-child(3n+1) {
    height: 45vh;
  }

  .water:nth-child(3n+2) {
    height: 50vh;
  }
}
</style>
