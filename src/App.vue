<script setup>
import { reactive, ref, onMounted, watch, computed } from 'vue'
import TabBar from '@/components/common/TabBar.vue'
import Home from '@/components/views/Home.vue'
import Message from '@/components/views/Message.vue'
import Publish from '@/components/views/Publish.vue'
import Video from '@/components/views/Video.vue'
import User from '@/components/views/User.vue'

const barList = [
  Home, Message, Publish, Video, User
]
let currentComp = ref(0)
function updateComp(index) {
  currentComp.value = index / 1
}

</script>

<template>
  <keep-alive>
    <component :is="barList[currentComp]"></component>
  </keep-alive>
  <TabBar @updateComp="updateComp"></TabBar>
</template>

<style lang="scss" scoped></style>
