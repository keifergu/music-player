<template>
  <div class="Audio">
      <audio id="audioTag" ref="audio"
        :loop="isLoop"
        src="http://www.w3school.com.cn/i/horse.ogg"
        @loadedmetadata="initInfo"
        @timeupdate="updateViewProgress"
        @ended="handledPlayEnd">
      </audio>
      <div class="play-progress">
        <span class="played-time">{{ playedTime | time }}</span>
        <slider class="played-slider" v-model="playedTime" :max="totalTime" :step="0.1" @change="changeAudioProgress"></slider>
        <span class="total-time">{{ totalTime | time }}</span>
      </div>
      <div class="controls">
          <span @click="isLoop = !isLoop">{{ isLoop ? "循环" : "单曲" }}</span>
          <span>上一首</span>
          <button class="play-pause" id="playPause" @click="isPlay = !isPlay">
            {{ isPlay ? "暂停" : "播放" }}
              <!-- <img class="icon-btn" :class="{'icon-play': !isPlay, 'icon-pause': isPlay}" > -->
          </button>
          <span>下一首</span>
      </div>
  </div>
</template>

<script>
import { Slider } from 'element-ui'

export default {
  name: 'HelloWorld',
  components: {
    Slider
  },
  data() {
    return {
      isLoop: true,
      isPlay: false,
      totalTime: 0,
      playedTime: 0,
      audioEl: null,
    };
  },
  mounted() {
    this.audioEl = this.$refs.audio
  },
  watch: {
    isPlay(newVal) {
      const { audioEl } = this
      if (newVal === true) audioEl.play()
      else if (newVal === false) audioEl.pause()
    }
  },
  methods: {
    initInfo() {
      const { audioEl } = this 
      this.totalTime = audioEl.duration
    },

    /**
     * @description 当播放时，更新当前播放进度数据
     */
    updateViewProgress() {
      const { audioEl } = this;
      const { currentTime } = audioEl;
      this.playedTime = currentTime;
    },

    /** 
     * @description 当进度条拖动时，更新当前的播放时间
     */
    changeAudioProgress(newTime) {
      const { audioEl } = this
      audioEl.currentTime = newTime
    },
    handledPlayEnd() {
      this.isPlay = false;
      this.playedTime = 0;
    },
  },
  filters: {
    time(s) {
      let t = '';
      if (s >= 0){
        var hour = Math.floor(s/3600);
        var min = Math.floor(s/60) % 60;
        var sec = s % 60;

        if (hour >= 10) {
          t = hour + ":";
        } else if (hour > 0 &&  hour < 10){
          t = '0'+ hour + ":";
        } 

        if (min < 10) t += "0";
        t += min + ":";
        if (sec < 10) t += "0";
        t += sec.toFixed(0);
      }
      return t;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.Audio{position: relative; width: 500px; margin: 0 auto;}
.pgs{width: 326px; margin: 0 auto 30px; background-color: #E3E8EE; text-align: center; position: relative; overflow: hidden; height: 35px;}
.pgs-play{position: absolute; top:0; left: 4.65%; width: 0; height: 100%; background-color: #4785f9; z-index: 1;}
.pgs img{width: 100%; position: relative; z-index: 2;}
.audio-name{position: absolute; top: 0; width: 100%; left: 0;text-align: center; color: #666; font-size: 12px;}
.controls{width: 100%; height: 40px; padding: 0; text-align: center;}
.play-pause{border: 0; outline: 0; padding: 0; width: 40px; height: 40px; margin: 0 28px; background: none; display: inline-block; vertical-align: middle;}
.icon-play{width: 100%; height: 100%; background: url(../images/play.png) no-repeat; display: block; color: #478f59;}
.icon-pause{width: 100%; height: 100%; background: url(../images/pause.png) no-repeat; display: block; color: #478f59;}
.controls span{color: #b3b5b7; font-size: 12px; display: inline-block; width: 44px;}
.audio-time{display: inline-block; vertical-align: middle;}

.play-progress {
  display: flex;
  align-items: center;
}

.played-time {
  display: inline-block;
  width: 50px;
}

.played-slider {
  flex: 1;
  margin: 0 20px;
}

.total-time {
  width: 50px;
}
</style>
