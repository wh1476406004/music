<script setup>
import {NIcon} from "naive-ui";
import {NProgress} from "naive-ui";
import MusicProgressBar from "@/components/MusicProgressBar.vue";
</script>
<template>
  <div class="bottom-bar">
    <audio ref="audio" :src="audioUrl" @timeupdate="handleTimeUpdate"></audio>
    <MusicProgressBar :currentTime="currentTime" :duration="duration" :on-seek="onSeek"></MusicProgressBar>
    <!--    上一首-->
    <div class="buttons">
      <n-icon size="50px" @click="last">
        <svg t="1679719286172" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
             p-id="1984" width="300" height="300">
          <path
              d="M511.8 64.6C265 64.6 64.2 265.3 64.2 512S265 959.6 511.8 959.6c246.7 0 447.6-200.8 447.6-447.6-0.1-246.7-200.9-447.4-447.6-447.4z m0 852.2C288.6 916.8 107 735.2 107 512c0-223.2 181.6-404.8 404.8-404.8C735 107.2 916.6 289 916.6 512c0 223.2-181.6 404.8-404.8 404.8z m0 0"
              fill="#76569A" p-id="1985"></path>
          <path
              d="M701.8 431.8c-13.2-7.9-29.7-8.1-43.2-0.6L441 551.6c-7.7 4.3-13.9 10.7-17.5 18.4V455.1c0-17.7-10.9-32-24.6-32-13.5 0-24.6 14.3-24.6 32v266.4c0 17.7 10.9 32 24.6 32 13.7 0 24.6-14.3 24.6-32V606.3c3.9 7.5 9.8 14.1 17.5 18.4l217.7 120.5c6.4 3.6 13.7 5.3 21.1 5.3 7.5 0 15.2-1.9 22-6 13-7.7 20.7-21.1 20.7-35.9v-241c0-14.7-7.7-28.1-20.7-35.8z"
              fill="#FFF433" p-id="1986"></path>
          <path
              d="M671.6 355.6c-13.2-7.9-29.7-8.1-43.2-0.6L410.7 475.5c-7.7 4.3-13.9 10.7-17.5 18.4v-115c0-17.7-10.9-32-24.6-32-13.5 0-24.6 14.3-24.6 32v266.4c0 17.7 10.9 32 24.6 32 13.7 0 24.6-14.3 24.6-32V530.2c3.9 7.5 9.8 14.1 17.5 18.4L628.4 669c6.4 3.6 13.7 5.3 21.1 5.3 7.5 0 15.2-1.9 22-6 13-7.7 20.7-21.1 20.7-35.9v-241c0.1-14.6-7.6-28.1-20.6-35.8z m-22 276c-0.2 0.2-0.2 0-0.4 0L433.2 512l216-119.6h0.4v239.2z m0 0"
              fill="#76569A" p-id="1987"></path>
        </svg>
      </n-icon>
      <!--      播放-->
      <n-icon size="50px" @click="play" v-if="!playFlag">
        <svg t="1679719603650" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
             p-id="2127" width="200" height="200">
          <path
              d="M570.5 662.2c-26.6-0.1-48.4-21.8-48.5-48.4l-0.8-213.3c-0.1-12.8 5-25.3 14-34.3 9.3-9 21.6-14 34.6-14 8.4 0 16.7 2.1 24 6.2l185 105.9c15.2 8.7 24.3 24.3 24.4 41.8 0.2 17.4-9 33.5-24 42.1L594.9 655.6c-7.4 4.3-15.8 6.6-24.4 6.6z"
              fill="#FFF433" p-id="2128"></path>
          <path
              d="M472.9 664.9c-26.6-0.1-48.4-21.8-48.5-48.4l-0.8-213.3c-0.1-12.8 5-25.3 14-34.3 9.3-9 21.6-14 34.6-14 8.4 0 16.7 2.1 24 6.2l185 105.9c15.2 8.7 24.3 24.3 24.4 41.8 0.2 17.4-9 33.5-24 42.1L497.2 658.3c-7.4 4.3-15.8 6.6-24.3 6.6z m1.3-245.7l0.7 181.2 10.7 6.1 156.6-91.2V503l-157.4-90-10.6 6.2z"
              fill="#76569A" p-id="2129"></path>
          <path
              d="M511.3 960.3c-247 0-448-201-448-448 0-247.1 201-448 448-448s448 201 448 448-200.9 448-448 448z m0-845.4C292.2 114.9 114 293.2 114 512.3c0 219.1 178.3 397.3 397.4 397.3 219.1 0 397.3-178.2 397.3-397.3 0-219.1-178.3-397.4-397.4-397.4z"
              fill="#76569A" p-id="2130"></path>
        </svg>
      </n-icon>
      <!--      暂停-->
      <n-icon size="50px" @click="pause" v-else>
        <svg t="1679719838160" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
             p-id="2417" width="200" height="200">
          <path
              d="M512 958.2C266 958.2 65.8 758.1 65.8 512 65.8 266 266 65.8 512 65.8S958.2 266 958.2 512c0 246.1-200.1 446.2-446.2 446.2z m0-841.6C294 116.6 116.6 294 116.6 512c0 218 177.4 395.4 395.4 395.4 218 0 395.4-177.4 395.4-395.4 0-218-177.4-395.4-395.4-395.4z"
              fill="#76569A" p-id="2418"></path>
          <path
              d="M511.4 655.5c0 26.4-21.4 47.8-47.8 47.8s-47.8-21.4-47.8-47.8v-287c0-26.4 21.4-47.8 47.8-47.8s47.8 21.4 47.8 47.8v287zM678.4 655.5c0 26.4-21.4 47.8-47.8 47.8s-47.8-21.4-47.8-47.8v-287c0-26.4 21.4-47.8 47.8-47.8s47.8 21.4 47.8 47.8v287z"
              fill="#FFF433" p-id="2419"></path>
          <path
              d="M595.5 679.1c-14.6 0-26.5-9.8-26.5-21.9V366.8c0-12.1 11.9-21.9 26.5-21.9s26.5 9.8 26.5 21.9v290.4c0 12.1-11.9 21.9-26.5 21.9z m-167 0c-14.6 0-26.5-9.8-26.5-21.9V366.8c0-12.1 11.9-21.9 26.5-21.9s26.5 9.8 26.5 21.9v290.4c0 12.1-11.9 21.9-26.5 21.9z"
              fill="#76569A" p-id="2420"></path>
        </svg>
      </n-icon>
      <!--      下一首-->
      <n-icon size="50px" @click="next">
        <svg t="1679719652044" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"
             p-id="2274" width="200" height="200">
          <path
              d="M511.8 64.2C265 64.2 64.2 264.8 64.2 511.6c0 246.7 200.8 447.6 447.6 447.6s447.6-200.8 447.6-447.6c-0.1-246.8-200.9-447.4-447.6-447.4z m0 852.2C288.6 916.4 107 734.8 107 511.6c0-223 181.6-404.8 404.8-404.8 223.2 0 404.8 181.6 404.8 404.8 0 223.2-181.6 404.8-404.8 404.8z m0 0"
              fill="#76569A" p-id="2275"></path>
          <path
              d="M321.7 431.4c-13 7.7-20.7 21.2-20.7 35.9v241c0 14.7 7.7 28.2 20.7 35.9 6.8 4.1 14.5 6 22 6s14.7-1.7 21.1-5.3l217.7-120.5c7.7-4.3 13.7-10.9 17.5-18.4v115.2c0 17.7 10.9 32 24.6 32 13.7 0 24.6-14.3 24.6-32V454.7c0-17.7-11.1-32-24.6-32-13.7 0-24.6 14.3-24.6 32v114.9c-3.6-7.7-9.8-14.1-17.5-18.4L364.9 430.7c-13.5-7.4-29.9-7.2-43.2 0.7z"
              fill="#FFF433" p-id="2276"></path>
          <path
              d="M352 355.2c-13 7.7-20.7 21.2-20.7 35.9v241c0 14.7 7.7 28.2 20.7 35.9 6.8 4.1 14.5 6 22 6s14.7-1.7 21.1-5.3l217.7-120.5c7.7-4.3 13.7-10.9 17.5-18.4v115.1c0 17.7 10.9 32 24.6 32 13.7 0 24.6-14.3 24.6-32V378.5c0-17.7-11.1-32-24.6-32-13.7 0-24.6 14.3-24.6 32v114.9c-3.6-7.7-9.8-14.1-17.5-18.4L395.1 354.6c-13.4-7.5-29.9-7.3-43.1 0.6z m22 36.7h0.4l216 119.6-216 119.6c-0.2 0-0.2 0.2-0.4 0V391.9z m0 239.3"
              fill="#76569A" p-id="2277"></path>
        </svg>
      </n-icon>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttons: [
        {id: 1, label: 'Button 1'},
        {id: 2, label: 'Button 2'},
        {id: 3, label: 'Button 3'},
        {id: 4, label: 'Button 4'},
        {id: 5, label: 'Button 5'},
      ],
      progress: 0,
      playFlag: false,
      percentage: 20,
      audioUrl: "your-audio-url.mp3",
      currentTime: 10,
      duration: 250

    }
  },
  methods: {
    last() {
      alert("上一首")
    },
    play() {
      this.playFlag = true
      this.percentage += 1;
    },
    pause() {
      this.playFlag = false
    },
    next() {
      alert("下一首")
    },
    handleTimeUpdate() {
      this.currentTime = this.$refs.audio.currentTime;
    },
    onSeek(time) {
      this.$refs.audio.currentTime = time;
    },
  }
};
</script>

<style>
.bottom-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 10px;
  box-shadow: 0px -2px 6px rgba(0, 0, 0, 0.1);
}

.buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-top: 10px;
}

button {
  padding: 10px;
  border: none;
  background-color: #e6e6e6;
  border-radius: 5px;
  cursor: pointer;
}

</style>