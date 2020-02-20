<template>
  <div class="base-timer">
    <svg class="base-timer__svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
      <g class="base-timer__circle">
        <circle class="base-timer__path-elapsed" cx="50" cy="50" r="45" />
        <path
          :stroke-dasharray="circleDasharray"
          class="base-timer__path-remaining"
          :class="remainingPathColor"
          d="
            M 50, 50
            m -45, 0
            a 45,45 0 1,0 90,0
            a 45,45 0 1,0 -90,0
          "
        />
      </g>
    </svg>
    <span class="base-timer__label">{{ formattedTimeLeft }}</span>
    <br />
    <select name="timer-setter" id="timer-setter" @change="selectedTime($event)">
      <option value selected disabled>---Select a Time---</option>
      <option v-for="second in seconds" :value="second.value" v-bind:key="second.id">{{second.text}}</option>
    </select>
    <button id="start" class="button is-dark is-large" @click="startTimer">GO</button>
    <div class="pause-or-kill">
      <!--     Pause Timer -->
      <button id="stop" class="button is-dark is-large" @click="pauseTimer">Pause</button>
      <!--     Restart Timer -->
      <button id="reset" class="button is-dark is-large" @click="restartTimer">Restart</button>
      <button id="kill" class="button" @click="kill">Reset</button>
    </div>
  </div>
</template>

<script>
let audio = new Audio(require("/assets/audio/ding.mp3"));

const FULL_DASH_ARRAY = 283;
const WARNING_THRESHOLD = 10;
const ALERT_THRESHOLD = 5;

const COLOR_CODES = {
  info: {
    color: "green"
  },
  warning: {
    color: "orange",
    threshold: WARNING_THRESHOLD
  },
  alert: {
    color: "red",
    threshold: ALERT_THRESHOLD
  }
};

let TIME_LIMIT = null;

export default {
  data() {
    return {
      timePassed: 0,
      timerInterval: null,
      seconds: [
        { id: 0, value: null, text: "--- Select a Time ---" },
        { id: 1, value: 20, text: "20 Seconds" },
        { id: 2, value: 30, text: "30 Seconds" },
        { id: 3, value: 60, text: "1 Minute" },
        { id: 4, value: 120, text: "2 Minutes" },
        { id: 5, value: 300, text: "5 Minutes" }
      ]
    };
  },

  computed: {
    circleDasharray() {
      return `${(this.timeFraction * FULL_DASH_ARRAY).toFixed(0)} 283`;
    },

    formattedTimeLeft() {
      const timeLeft = this.timeLeft;
      const minutes = Math.floor(timeLeft / 60);
      let seconds = timeLeft % 60;

      if (seconds < 10) {
        seconds = `0${seconds}`;
      }

      return `${minutes}:${seconds}`;
    },

    timeLeft() {
      return TIME_LIMIT - this.timePassed;
    },

    timeFraction() {
      const rawTimeFraction = this.timeLeft / TIME_LIMIT;
      return rawTimeFraction - (1 / TIME_LIMIT) * (1 - rawTimeFraction);
    },

    remainingPathColor() {
      const { alert, warning, info } = COLOR_CODES;

      if (this.timeLeft <= alert.threshold) {
        return alert.color;
      } else if (this.timeLeft <= warning.threshold) {
        return warning.color;
      } else {
        return info.color;
      }
    }
  },

  watch: {
    timeLeft(newValue) {
      if (newValue === 0) {
        this.onTimesUp();
      }
    }
  },

  // startTimer() {
  //   this.startTimer();
  // },

  methods: {
    onTimesUp() {
      clearInterval(this.timerInterval);
      audio.play();
      setTimeout(function() {
        location.reload();
      }, 2000);
    },
    startTimer() {
      this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
    },
    restartTimer() {
      clearInterval(this.timerInterval);
      this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
    },
    pauseTimer() {
      clearInterval(this.timerInterval);
    },
    kill() {
      clearInterval(this.timerInterval);
      location.reload();
    },
    selectedTime(event) {
      console.log(event.target.value);
      TIME_LIMIT = event.target.value;
    }
  }
};
</script>

<style scoped lang="scss">
.base-timer {
  position: relative;
  width: 300px;
  height: 300px;

  &__svg {
    transform: scaleX(-1);
  }

  &__circle {
    fill: none;
    stroke: none;
  }

  &__path-elapsed {
    stroke-width: 7px;
    stroke: grey;
  }

  &__path-remaining {
    stroke-width: 7px;
    stroke-linecap: round;
    transform: rotate(90deg);
    transform-origin: center;
    transition: 1s linear all;
    fill-rule: nonzero;
    stroke: currentColor;

    &.green {
      color: rgb(65, 184, 131);
    }

    &.orange {
      color: orange;
    }

    &.red {
      color: red;
    }
  }

  &__label {
    position: absolute;
    width: 300px;
    height: 300px;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 48px;
  }
}

select {
  margin-top: 30px;
  margin-right: 15px;
  padding: 15px;
  border-radius: 8px;
  background-color: rgb(65, 184, 131);
  color: #fff;
  font-size: 18px;
  -webkit-box-shadow: 10px 10px 20px -8px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 10px 10px 20px -8px rgba(0, 0, 0, 0.75);
  box-shadow: 10px 10px 20px -8px rgba(0, 0, 0, 0.75);
}

button {
  padding: 15px;
  font-size: 18px;
  background-color: rgb(65, 184, 131);
  border-radius: 8px;
  color: #fff;
  -webkit-box-shadow: 10px 10px 20px -8px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 10px 10px 20px -8px rgba(0, 0, 0, 0.75);
  box-shadow: 10px 10px 20px -8px rgba(0, 0, 0, 0.75);
  outline: none;
}
.pause-or-kill {
  margin-top: 20px;
}
</style>
