<template>
  <div class="dashboard">
    <h1>我的大屏内容</h1>

    <FullscreenToggle
        ref="fullscreenRef"
        targetSelector=".dashboard"
        :magnifyIcon="magnifyImg"
        :shrinkIcon="shrinkImg"
        @change="onFullscreenChange"
    />
  </div>

  <button @click="goFullscreenByCode">代码触发全屏</button>
</template>

<script setup>
import { ref } from 'vue'
import FullscreenToggle from '@/view/FullscreenToggle.vue'

// 图标路径
const magnifyImg = new URL('/magnify.png', import.meta.url).href
const shrinkImg = new URL('/shrink.png', import.meta.url).href

// 引用组件实例
const fullscreenRef = ref(null)

// 全屏状态变化回调
const onFullscreenChange = (status) => {
  console.log('全屏状态:', status)
}

// 通过代码触发全屏
const goFullscreenByCode = () => {
  fullscreenRef.value.toggleFullscreen()
}
</script>

<style>
.dashboard {
  background: #111;
  color: #fff;
  padding: 20px;
  height: 300px;
}
</style>
