<template>
  <div class="fullscreen-btn" @click="toggleFullscreen">
    <img
        :src="isFullscreen ? shrink : magnify"
        alt="全屏切换"
        class="fullscreen-icon"
    />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, defineProps } from 'vue';
import shrink from '/shrink.png'
import magnify from '/magnify.png'

// 定义 props 接收传入的 elementId
const props = defineProps({
  elementId: {
    type: String,
    required: false,
    default: ''
  }
});

// 全屏状态
const isFullscreen = ref(false);

// 全屏功能
const toggleFullscreen = () => {
  let targetElement = null;

  // 如果传入了 elementId，使用该 id 查找对应的元素
  if (props.elementId) {
    targetElement = document.getElementById(props.elementId);
  } else {
    // 否则默认使用 .dashboard 元素
    targetElement = document.querySelector('.dashboard');
  }

  if (targetElement) {
    if (!isFullscreen.value) {
      // 进入全屏
      if (targetElement.requestFullscreen) {
        targetElement.requestFullscreen();
      } else if (targetElement.webkitRequestFullscreen) {
        targetElement.webkitRequestFullscreen();
      } else if (targetElement.msRequestFullscreen) {
        targetElement.msRequestFullscreen();
      } else if (targetElement.mozRequestFullScreen) {
        targetElement.mozRequestFullScreen();
      }
    } else {
      // 退出全屏
      if (document.exitFullscreen) {
        document.exitFullscreen();
      } else if (document.webkitExitFullscreen) {
        document.webkitExitFullscreen();
      } else if (document.msExitFullscreen) {
        document.msExitFullscreen();
      } else if (document.mozCancelFullScreen) {
        document.mozCancelFullScreen();
      }
    }
  }
};

// 监听全屏状态变化
const handleFullscreenChange = () => {
  const fullscreenElement = document.fullscreenElement ||
      document.webkitFullscreenElement ||
      document.msFullscreenElement ||
      document.mozFullScreenElement;

  isFullscreen.value = !!fullscreenElement;
};

onMounted(() => {
  document.addEventListener('fullscreenchange', handleFullscreenChange);
  document.addEventListener('webkitfullscreenchange', handleFullscreenChange);
  document.addEventListener('mozfullscreenchange', handleFullscreenChange);
  document.addEventListener('MSFullscreenChange', handleFullscreenChange);
});

onUnmounted(() => {
  // 移除全屏状态监听
  document.removeEventListener('fullscreenchange', handleFullscreenChange);
  document.removeEventListener('webkitfullscreenchange', handleFullscreenChange);
  document.removeEventListener('mozfullscreenchange', handleFullscreenChange);
  document.removeEventListener('MSFullscreenChange', handleFullscreenChange);
});
</script>

<style scoped>
.fullscreen-icon {
  width: 24px;
  height: 24px;
  transition: transform 0.3s ease;
}

.fullscreen-btn:hover .fullscreen-icon {
  transform: scale(1.1);
}
</style>
