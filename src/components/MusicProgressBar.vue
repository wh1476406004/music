<template>
  <div class="music-progress-bar">
    <div class="progress" ref="progress" @click="handleClick">
      <div class="thumb" ref="thumb" id="thumb" :style="{ left: thumbLeft }"></div>
    </div>
    <div class="time-info">
      <span class="current-time">{{ currentTime }}</span>
      <span class="total-time">{{ totalTime }}</span>
    </div>
  </div>
</template>

<script>
import {computed, onMounted, onUnmounted, ref} from "vue";

export default {
  name: "MusicProgressBar",
  props: {
    duration: {
      type: Number,
      required: true,
    },
    currentTime: {
      type: Number,
      required: true,
    },
    onSeek: {
      type: Function,
      required: true,
    }
  },
  setup(props) {
    const progress = ref(null);
    const thumb = ref(null);
    const isDragging = ref(false);
    const currentTime = computed(() => formatTime(props.currentTime));
    const totalTime = computed(() => formatTime(props.duration));
    const thumbLeft = computed(() => {
      if (!progress.value) return "0";
      return `${(props.currentTime / props.duration) * progress.value.clientWidth}px`;
    });
    function formatTime(time) {
      const minutes = Math.floor(time / 60)
          .toString()
          .padStart(2, "0");
      const seconds = Math.floor(time % 60)
          .toString()
          .padStart(2, "0");
      return `${minutes}:${seconds}`;
    }

    function handleMouseDown() {
      isDragging.value = true;
    }

    function handleMouseUp() {
      isDragging.value = false;
    }

    function handleClick(event) {
      const {clientX} = event;
      const {left, width} = progress.value.getBoundingClientRect();
      const ratio = (clientX - left) / width;
      props.onSeek(ratio * props.duration);
    }

    function handleMouseMove(event) {
      if (isDragging.value) {
        const {clientX} = event;
        const {left, width} = progress.value.getBoundingClientRect();
        let ratio = (clientX - left) / width;
        if (ratio < 0) ratio = 0;
        if (ratio > 1) ratio = 1;
        props.onSeek(ratio * props.duration);
      }
    }

    onMounted(() => {
      thumb.value.addEventListener("mousedown", handleMouseDown);
      document.addEventListener("mouseup", handleMouseUp);
      document.addEventListener("mousemove", handleMouseMove);
    });

    onUnmounted(() => {
      thumb.value.removeEventListener("mousedown", handleMouseDown);
      document.removeEventListener("mouseup", handleMouseUp);
      document.removeEventListener("mousemove", handleMouseMove);
    });

    return {
      progress,
      thumb,
      isDragging,
      currentTime,
      totalTime,
      thumbLeft,
      handleClick
    };
  },
};
</script>

<style scoped>
.music-progress-bar {
  width: 100%;
  height: 50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}

.progress {
  width: 80%;
  height: 2px;
  background-color: #f0f0f0;
  position: relative;
  cursor: pointer;
}

.thumb {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background-color: #000;
  position: absolute;
  top: -5px;
  transform: translateX(-50%);
  cursor: pointer;
}

.time-info {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 80%;
}

.current-time,
.total-time {
  font-size: 14px;
  color: #666;
  font-family: Arial, sans-serif;
}

</style>
