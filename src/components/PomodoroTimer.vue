<template>
  <!-- our template -->
  <section id="app" class="hero is-info is-fullheight is-bold">
    <div class="hero-body">
      <div class="container has-text-centered">
        <h2 class="title is-6">{{title}}</h2>

        <div id="timer">
          <span id="minutes">{{ minutes }}</span>
          <span id="middle">:</span>
          <span id="seconds">{{ seconds }}</span>
        </div>

        <div id="buttons">
          <!--     Start TImer -->
          <button id="start" class="button is-dark is-large" v-if="!timer" @click="startTimer">
            <i class="far fa-play-circle"></i>
          </button>
          <!--     Pause Timer -->
          <button id="stop" class="button is-dark is-large" v-if="timer" @click="stopTimer">
            <i class="far fa-pause-circle"></i>
          </button>
          <!--     Restart Timer -->
          <button id="reset" class="button is-dark is-large" v-if="resetButton" @click="resetTimer">
            <i class="fas fa-undo"></i>
          </button>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
const app = new Vue({
  el: "#app",
  data: {
    defaultTime: 60 * 25,
    time: 0,
    timer: null
  },
  created() {
    this.time = this.defaultTime;
  },
  methods: {
    formatNumber(num) {
      return (num < 10 ? "0" : "") + num.toString();
    },
    getMinutes() {
      return this.formatNumber(Math.floor(this.time / 60));
    },
    getSeconds() {
      return this.formatNumber(this.time % 60);
    },
    playPause() {
      if (!this.timer) {
        this.play();
      } else {
        this.pause();
      }
    },
    play() {
      if (this.time <= 0) this.time = this.defaultTime;
      this.timer = setInterval(() => {
        this.time -= 1;
        if (this.time <= 0) this.playPause();
      }, 1000);
    },
    pause() {
      clearInterval(this.timer);
      this.timer = null;
    },
    reset() {
      this.time = this.defaultTime;
    },
    getPlayState() {
      return !!this.timer;
    }
  }
});
</script>

<style scoped lang="scss">
#message {
  color: #ddd;
  font-size: 50px;
  margin-bottom: 20px;
}

#timer {
  font-size: 200px;
  line-height: 1;
  margin-bottom: 40px;
}
</style>
