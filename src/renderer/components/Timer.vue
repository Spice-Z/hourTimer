<template>
<div class="all">
  <timer-line :remain-min="remainMin" :remainSec="remainSec"></timer-line>
  <button class="all__button-start" v-if="!isCount" @click='startTimer'></button>
  <button class="all__button-stop" v-if="isCount" @click='stopTimer'></button>
  <button class="all__button-reset" v-if="!isCount" @click='resetTimer'></button>
</div>
</template>

<script>
import TimerLine from "@/components/TimerLine";

export default {
  name: "timer",
  components: {
    TimerLine
  },
  data: function() {
    return {
      remainTime: 3600000,
      nowTime: 0,
      diffTime: 0,
      startedTime: 0,
      isCount: false
    };
  },
  created: function() {
    let time = localStorage.getItem("remainTime");
    if (time) {
      this.remainTime = time;
    }
  },
  methods: {
    startTimer: function() {
      let vm = this;
      if (this.remainTime == 0) {
        this.remainTime = 3600000;
      }
      vm.isCount = true;
      vm.startedTime = Math.floor(performance.now() - vm.diffTime);
      (function loop() {
        let loopfps = vm.diffTime;
        vm.nowTime = Math.floor(performance.now());
        vm.diffTime = vm.nowTime - vm.startedTime;
        vm.remainTime -= vm.diffTime - loopfps;
        if (vm.remainTime % 10000 < 20) {
          localStorage.setItem("remainTime", vm.remainTime);
        }
        if (vm.remainTime <= 0) {
          vm.finishTimer();
        }
        vm.animateFrame = requestAnimationFrame(loop);
      })();
    },
    stopTimer: function() {
      this.isCount = false;
      cancelAnimationFrame(this.animateFrame);
      localStorage.setItem("remainTime", this.remainTime);
    },
    finishTimer: function() {
      this.stopTimer();
      this.remainTime = 0;
      localStorage.setItem("remainTime", this.remainTime);
    },
    resetTimer: function() {
      this.remainTime = 3600000;
      localStorage.setItem("remainTime", this.remainTime);
    }
  },
  computed: {
    computedRemainTime: function() {
      return this.remainTime;
    },
    remainMin: function() {
      if (this.remainTime == 3600000) {
        return 60;
      }
      return Math.floor(this.remainTime / 1000 / 60) % 60;
    },
    remainSec: function() {
      return Math.floor(this.remainTime / 1000) % 60;
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  width: 100%;
}

.all {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.all__button-start,
.all__button-stop {
  font-size: 18px;
  width: 100px;
  height: 50px;
  border-radius: 15px;
  border: none;
  outline: none;
  background: #1d1c22;
}

.all__button-start {
  background-image: url(../assets/play.svg);
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}

.all__button-stop {
  background-image: url(../assets/stop.svg);
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}

.all__button-reset {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 50px;
  height: 50px;
  border: none;
  outline: none;
  background: none;
  background-image: url(../assets/reset.svg);
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  transition: 0.2s;
  transform: rotateY(0deg);
}

.all__button-reset:active {
  transform: rotateY(360deg);
}
</style>
