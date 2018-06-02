<template>
<div id="wrapper">
  <p>{{ remainMin }}:{{ remainSec }}</p>
  <button v-if="!isCount" @click='startTimer'>start</button>
  <button v-if="isCount" @click='stopTimer'>stop</button>
  <timer-line :remain-min="remainMin" :remainSec="remainSec"></timer-line>
</div>
</template>

<script>
import Pie from "@/components/Pie";
import TimerLine from "@/components/TimerLine";

export default {
  name: "timer",
  components: {
    Pie,
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
      console.log("stopTimer");
      this.isCount = false;
      cancelAnimationFrame(this.animateFrame);
      localStorage.setItem("remainTime", this.remainTime);
    },
    finishTimer: function() {
      this.stopTimer();
      this.remainTime = 0;
      localStorage.setItem("remainTime", this.remainTime);
    }
  },
  computed: {
    computedRemainTime: function() {
      return this.remainTime;
    },
    remainMin: function() {
      return Math.floor(this.remainTime / 1000 / 60) % 60;
    },
    remainSec: function() {
      return Math.floor(this.remainTime / 1000) % 60;
    },
    chartData: function() {
      let remainMinForChart = this.remainMin;
      return {
        datasets: [
          {
            data: [60 - this.remainMin, this.remainMin],
            backgroundColor: ["#DDDFBD", "#F8D32F"]
          }
        ],
        labels: ["消費時間", "残り時間"]
      };
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
</style>
