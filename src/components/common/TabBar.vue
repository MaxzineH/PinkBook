<script setup>
import { reactive, ref } from 'vue'

const barList = reactive([
  {
    id: "Home",
    name: "首页",
  },
  {
    id: "Video",
    name: "视频",
  },
  {
    id: "Pulish",
    name: "发布",
  },
  {
    id: "Message",
    name: "消息",
  },
  {
    id: "User",
    name: "我",
  },
])

const emits = defineEmits(['updateComp'])
let currentIdx = ref(0)
function tabOn(index) {
  currentIdx.value = index
  emits('updateComp', index)
}


</script>

<template>
  <div class="bar-content">
    <div v-for="(item, idx) in barList" :key="idx" @click="tabOn(idx)" :class="{ 'bar-active': idx == currentIdx }">
      {{ item.name }}
    </div>
  </div>
</template>

<style lang="scss" scoped>
.bar-content {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  height: 3rem;
  background-color: rgba(254, 254, 254);
  @include flexcc;

  div {
    font-size: 1rem;
    color: rgba(149, 149, 149);
    text-align: center;
    width: 20%;
    transition: 0.1s linear;
  }

  .bar-active {
    font-size: 1.05rem !important;
    color: rgba(51, 51, 51) !important;
    font-weight: bold;
  }
}
</style>
