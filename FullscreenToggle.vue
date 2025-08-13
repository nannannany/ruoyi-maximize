<template>
  <img
      :src="isFullscreen ? shrinkIcon : magnifyIcon"
      alt="全屏切换"
      class="fullscreen-icon"
      @click="toggleFullscreen"
  />
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, defineExpose } from 'vue'

/**
 * Props
 * @param {String} targetSelector - 需要全屏的容器 CSS 选择器
 * @param {String} magnifyIcon - 放大图标路径
 * @param {String} shrinkIcon - 缩小图标路径
 */
const props = defineProps({
  targetSelector: { type: String, required: true },
  magnifyIcon: { type: String, required: true },
  shrinkIcon: { type: String, required: true }
})

/**
 * Emits
 * @event change - 全屏状态变化，参数为 true/false
 */
const emit = defineEmits(['change'])

const isFullscreen = ref(false)

/**
 * 切换全屏
 */
const toggleFullscreen = () => {
  const targetElement = document.querySelector(props.targetSelector)
  if (!targetElement) {
    console.warn(`未找到元素: ${props.targetSelector}`)
    return
  }

  if (!isFullscreen.value) {
    if (targetElement.requestFullscreen) {
      targetElement.requestFullscreen()
    } else if (targetElement.webkitRequestFullscreen) {
      targetElement.webkitRequestFullscreen()
    } else if (targetElement.msRequestFullscreen) {
      targetElement.msRequestFullscreen()
    } else if (targetElement.mozRequestFullScreen) {
      targetElement.mozRequestFullScreen()
    }
  } else {
    if (document.exitFullscreen) {
      document.exitFullscreen()
    } else if (document.webkitExitFullscreen) {
      document.webkitExitFullscreen()
    } else if (document.msExitFullscreen) {
      document.msExitFullscreen()
    } else if (document.mozCancelFullScreen) {
      document.mozCancelFullScreen()
    }
  }
}

/**
 * 监听全屏状态变化
 */
const handleFullscreenChange = () => {
  const fullscreenElement =
      document.fullscreenElement ||
      document.webkitFullscreenElement ||
      document.msFullscreenElement ||
      document.mozFullScreenElement

  isFullscreen.value = !!fullscreenElement
  emit('change', isFullscreen.value)
}

onMounted(() => {
  document.addEventListener('fullscreenchange', handleFullscreenChange)
  document.addEventListener('webkitfullscreenchange', handleFullscreenChange)
  document.addEventListener('msfullscreenchange', handleFullscreenChange)
  document.addEventListener('mozfullscreenchange', handleFullscreenChange)
})

onBeforeUnmount(() => {
  document.removeEventListener('fullscreenchange', handleFullscreenChange)
  document.removeEventListener('webkitfullscreenchange', handleFullscreenChange)
  document.removeEventListener('msfullscreenchange', handleFullscreenChange)
  document.removeEventListener('mozfullscreenchange', handleFullscreenChange)
})

/**
 * 对外暴露方法
 */
defineExpose({
  toggleFullscreen,
  isFullscreen
})
</script>

<style scoped>
.fullscreen-icon {
  cursor: pointer;
  width: 24px;
  height: 24px;
}
</style>
