<template>
  <div class="app-container">
    <div class="song-list-container">
      <song-list2 />
    </div>


    <div class="main-container">
      <div class="blank-container"></div>
      <div class="player-container">
        <BottomBar></BottomBar>
      </div>
      <div class="blank-container"></div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
// import SongList from './components/SongList.vue'
import BottomBar from "@/components/bottomBar.vue";
import SongList2 from "@/components/songList2.vue";
export default defineComponent({
  name: 'App',
  components: {
    BottomBar,SongList2
  }
})
</script>

<style>
/* 根容器 */
.app-container {
  display: flex;
  flex-direction: row;
  height: 100vh;
}

/* 歌曲列表容器 */
.song-list-container {
  width: 300px;
  border-right: 1px solid #ccc;
}

/* 主要内容容器 */
.main-container {
  flex: 1;
  display: flex;
  flex-direction: column;
}

/* 留白容器 */
.blank-container {
  flex: 1;
}

/* 播放器容器 */
.player-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 300px;
}

/* 歌曲信息容器 */
.song-info {
  width: 400px;
  height: 200px;
  border: 1px solid #ccc;
  margin-bottom: 20px;
}

/* 进度条容器 */
.progress-bar {
  width: 400px;
  height: 20px;
  border: 1px solid #ccc;
  margin-bottom: 20px;
}

/* 控制按钮容器 */
.controls {
  width: 200px;
  height: 50px;
  display: flex;
  justify-content: space-between;
}
</style>
